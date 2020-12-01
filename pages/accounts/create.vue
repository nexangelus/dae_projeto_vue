<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <div class="jumbotron">
      <h2>Create new Account</h2>
    </div>
    <form>
      <div v-if="!this.isUpdate" class="form-group">
        <label for="email">Email</label>
        <input id="email" class="form-control" v-model="email">
      </div>
      <div class="form-group">
        <label for="group">Group</label>
        <select class="form-control" id="group" v-model="group">
          <option value="Admin" v-if="$auth.user.groups.includes('Admin')">Admin</option>
          <option value="Designer" v-if="$auth.user.groups.includes('Admin')">Designer</option>
          <option value="Client">Client</option>
        </select>
      </div>
      <button type="reset" class="btn btn-danger">Clear</button>
      <button @click.prevent="create" class="btn btn-primary">Create</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      email: null,
      group: null,
      items: [{
        text: 'Dashboard',
        to: { name: 'dashboard' }
      }, {
        text: 'Account Create',
        to: { name: 'accounts' }
      },]
    }
  },
  methods: {
    create() {
      this.$axios.$post("/api/accounts", {
          email: this.email,
          group: this.group,
        })
        .then(response => {
            this.$toast.success("Email Sent")
        });

    },
  }
}
</script>

<style scoped>

</style>
