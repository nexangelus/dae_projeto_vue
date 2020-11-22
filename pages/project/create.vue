<template>
  <div>
    <Create/>
  </div>
</template>

<script>
import Create from "@/components/project/create";
export default {
  components: {Create},
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