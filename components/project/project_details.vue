<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <div class="jumbotron">
      <h2>Project Details</h2>
    </div>
    <table class="table table-striped">
      <tbody>
      <tr>
        <th scope="row">Client</th>
        <td>{{ project.clientUsername }}</td>
      </tr>
      <tr>
        <th scope="row">Designer</th>
        <td>{{ project.designerUsername }}</td>
      </tr>
      <tr>
        <th scope="row">Title</th>
        <td>{{ project.title }}</td>
      </tr>
      <tr>
        <th scope="row">Description</th>
        <td>{{ project.description }}</td>
      </tr>
      <tr>
        <th scope="row">Created</th>
        <td>{{ $moment(project.created).format("L LT") }}</td>
      </tr>
      <tr v-if="project.updated">
        <th scope="row">Updated</th>
        <td>{{ $moment(project.updated).format("L LT") }}</td>
      </tr>
      </tbody>
    </table>
    <nuxt-link class="btn btn-primary" :to="`/project/${project.id}/update/`">Update Project Details</nuxt-link>
    <nuxt-link class="btn btn-primary" :to="`/project/${project.id}/structure/simulate`" v-if="$auth.user.groups.includes('Designer')">Simulate</nuxt-link>
    <nuxt-link class="btn btn-primary" :to="`/project/${project.id}/upload`" v-if="$auth.user.groups.includes('Client')">Upload Photos</nuxt-link>
  </div>
</template>

<script>
export default {
  name: "project-details",
  data: function () {
    return {
      project: {},
      items: [{
        text: 'Dashboard',
        to: { name: 'dashboard' }
      }, {
        text: 'Project Details',
        active: true
      }]
    }
  },
  computed: {
    id () {
      return this.$route.params.id
    }
  },
  mounted() {
    //const user = this.username == "me" ? this.$auth.user.sub : this.username
    this.$axios.$get(`/api/projects/${this.id}`).then((response) => {
      this.project = response
    })
  }
}
</script>

<style scoped>

</style>