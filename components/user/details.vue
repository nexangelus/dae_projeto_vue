<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <div class="jumbotron">
      <h2>User Details</h2>
    </div>
    <table class="table table-striped">
      <tbody>
      <tr>
        <th scope="row">Name</th>
        <td>{{ user.name }}</td>
      </tr>
      <tr>
        <th scope="row">Email</th>
        <td>{{ user.email }}</td>
      </tr>
      <tr v-if="user.address">
        <th scope="row">Address</th>
        <td>{{ user.address }}</td>
      </tr>
      <tr v-if="user.contact">
        <th scope="row">Contact</th>
        <td>{{ user.contact }}</td>
      </tr>
      <tr>
        <th scope="row">Created</th>
        <td>{{ $moment(user.created).format("DD/MM/YYYY") }}</td>
      </tr>
      <tr v-if="user.updated">
        <th scope="row">Updated</th>
        <td>{{ $moment(user.updated).format("DD/MM/YYYY") }}</td>
      </tr>
      </tbody>
    </table>
    <nuxt-link class="btn btn-primary" :to="`/${$auth.user.groups[0].toLowerCase()}/${username}/update/`">Update Details</nuxt-link>
  </div>
</template>
<script>
export default {
  data() {
    return {
      user: [],
      username: this.$route.params.username,
      type: this.$auth.user.groups.includes("client"),
      items: [{
        text: 'Dashboard',
        to: { name: 'dashboard' }
      }, {
        text: 'User Details',
        active: true
      }]
    };
  },
  mounted() {
    this.$axios
      .$get(`/api/${this.$auth.user.groups[0].toLowerCase()}s/${this.username}`
      )
      .then((user) => {
        this.user = user;
      });
  },
};
</script>
