<template>
  <div>
    <h1>Olá {{ designer.name }}</h1>
    <projects_list :projects="projects"/>
  </div>
</template>

<script>
import Projects_list from "@/components/projects_list";

export default {
  components: {Projects_list},
  data: function () {
    return {
      designer: {name: ""},
      projects: null
    }
  },
  name: "dash-designer",
  computed: {
    username () {
      return this.$auth.user.sub
    }
  },
  mounted() {
    //const user = this.username == "me" ? this.$auth.user.sub : this.username
    this.$axios.$get(`/api/designers/${this.username}`).then((response) => {
      this.designer = response;
    })
    this.$axios.$get(`/api/designers/${this.username}/projects`).then((response) => {
      this.projects = response;
    })
  }
}
</script>

<style scoped>

</style>
