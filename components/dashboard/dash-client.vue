<template>
  <div>
    <h1>Olá {{client.name}}</h1>
    <projects_list :projects="projects"/>
  </div>
</template>

<script>
import Projects_list from "@/components/projects_list";

export default {
  components: {Projects_list},
  name: "dash-client",
  data: function () {
    return {
      client: {name:""},
      projects: null
    }
  },
  computed: {
    username () {
      return this.$auth.user.sub
    }
  },
  mounted() {
    //const user = this.username == "me" ? this.$auth.user.sub : this.username
    this.$axios.$get(`/api/clients/${this.username}`).then((response) => {
      this.client = response;
    })
    this.$axios.$get(`/api/clients/${this.username}/projects`).then((response) => {
      this.projects = response;
    })
  }
}
</script>

<style scoped>

</style>
