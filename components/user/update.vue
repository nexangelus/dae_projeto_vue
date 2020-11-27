<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <div class="jumbotron">
      <h2>Update Details</h2>
    </div>
    <form>
      <form>
        <div class="form-group">
          <label for="name">Name</label>
          <input v-model="user.name" type="text" class="form-control" id="name" aria-describedby="emailHelp">
        </div>
        <div class="form-group">
          <label for="email">Email</label>
          <input v-model="user.email" type="email" class="form-control" id="email">
        </div>
        <div class="form-group" v-if="type">
          <label for="address">Address</label>
          <input v-model="user.address" type="text" class="form-control" id="address">
        </div>
        <div class="form-group" v-if="type">
          <label for="contact">Contact</label>
          <input v-model="user.contact" type="text" class="form-control" id="contact">
        </div>
        <div class="group form-group">
          <label><strong>Change Password</strong></label>
          <div class="form-group">
            <label for="oldPassword">Previous Password</label>
            <input v-model="oldPassword" type="password" class="form-control" id="oldPassword">
          </div>
          <div class="form-group">
            <label for="newPassword">New Password</label>
            <input v-model="newPassword" type="password" class="form-control" id="newPassword">
          </div>
        </div>
        <button @click.prevent="cancel" class="btn btn-danger">Cancel</button>
        <button @click.prevent="update" class="btn btn-primary">Update</button>
      </form>
    </form>
  </div>

</template>
<script>
export default {
  data() {
    return {
      user: [],
      group: this.$auth.user.groups[0].toLowerCase(),
      oldPassword: '',
      newPassword: '',
      type: this.$auth.user.groups.includes('Client') || this.$auth.user.groups.includes('Manufacturer'),
      items: [{
        text: 'Dashboard',
        to: { name: 'dashboard' }
      }, {
        text: 'User Details',
        to: `/${this.$auth.user.groups[0].toLowerCase()}/${this.$auth.user.sub}`
      }, {
        text: 'Edit',
        active: true
      }]
    };
  },
  mounted() {
    this.$axios.$get(`/api/${this.group}s/${this.$auth.user.sub}`).then((user) => {
      this.user = user
    });
  },
  methods: {
    update() {
      const data = {
        username: this.user.username,
        newPassword: this.newPassword,
        oldPassword: this.oldPassword,
        name: this.user.name,
        email: this.user.email
      }
      if (this.type) {
        data.address = this.user.address;
        data.contact = this.user.contact;
      }
      this.$axios
        .$put(`/api/${this.group}s/${this.$auth.user.sub}`, data)
        .then(() => {
          this.$router.push(`/${this.group}/${this.$auth.user.sub}/`);
        });
    },
    cancel() {
      this.$router.push(`/${this.group}/${this.$auth.user.sub}/`)
    }
  },
};
</script>
<style>
.group {
  border: 1px solid #ced4da;
  border-radius: 0.25rem;
  padding: 0.375rem 0.75rem;
}
</style>
