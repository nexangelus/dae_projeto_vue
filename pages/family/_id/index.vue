<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <div class="jumbotron">
      <h2>Family Details</h2>
    </div>
    <table class="table table-striped">
      <tbody>
      <tr>
        <th scope="row">Name</th>
        <td>{{ family.name }}</td>
      </tr>
      <tr>
        <th scope="row">Created</th>
        <td>{{ $moment(family.created).format("L LT") }}</td>
      </tr>
      <tr v-if="family.updated">
        <th scope="row">Updated</th>
        <td>{{ $moment(family.updated).format("L LT") }}</td>
      </tr>
      </tbody>
    </table>
    <nuxt-link class="btn btn-primary" v-if="family.manufacturerUsername === this.$auth.user.sub" :to="`/family/${family.id}/update`">Update Family</nuxt-link>
  </div>
</template>

<script>
export default {
  data() {
    return {
      family: [],
      items: [{
        text: 'Dashboard',
        to: { name: 'dashboard' }
      }, {
        text: 'Families',
        to: { name: 'family' }
      }, {
        text: `${this.$route.params.id}`,
        active: true
      }]
    };
  },
  computed: {
    id () {
      return this.$route.params.id
    }
  },
  created() {
    this.$axios.$get(`/api/families/${this.id}`).then((family) => {
      this.family = family;
    });
  },
};
</script>

<style scoped>

</style>
