<template>
  <div>
    <projects_list :projects="projects"/>
  </div>
</template>

<script>
import Projects_list from "@/components/projects_list";
export default {
  components: {Projects_list},
  data: function () {
    return {
      projects: null
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