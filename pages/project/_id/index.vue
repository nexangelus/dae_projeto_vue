<template>
    <b-container>
    <h4>Project Details</h4>
    <p>Client: {{ project.clientUsername }}</p>
    <p>Designer: {{ project.designerUsername }}</p>
    <p>Title: {{ project.title }}</p>
    <p>Description: {{ project.description }}</p>
    <p>Created: {{ project.created }}</p>
    <nuxt-link :to="`/project/${project.id}/upload_photos`" v-if="$auth.user.groups.includes('Client')">UPLOAD PHOTOS</nuxt-link>
    <nuxt-link :to="`/project/${project.id}/upload_photos`" v-if="$auth.user.groups.includes('Designer')">Simulate</nuxt-link>
  </b-container>
</template>

<script>
export default {
  data: function () {
    return {
      project: {}
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