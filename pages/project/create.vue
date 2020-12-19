<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <Create/>
  </div>
</template>

<script>
import Create from "@/components/project/create";
export default {
  components: {Create},
  name: 'project-create',
  data: function () {
    return {
      projects: null,
      items: [{
        text: 'Dashboard',
        to: {name: 'dashboard'}
      }, {
        text: 'Create Project',
        active: true
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
