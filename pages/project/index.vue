<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <projects_list :projects="projects"/>
  </div>
</template>

<script>
import Projects_list from "@/components/projects_list";
export default {
  name: 'project',
  components: {Projects_list},
  data: function () {
    return {
      projects: null,
      items: [{
        text: 'Dashboard',
        to: { name: 'dashboard' }
      }, {
        text: 'Project',
        to: { name: 'project' }
      }]
    }
  },
  computed: {
  },
  mounted() {
    //const user = this.username == "me" ? this.$auth.user.sub : this.username
    this.$axios.$get(`/api/projects`).then((response) => {
      this.projects = response;
    })
  }
}
</script>

<style scoped>

</style>
