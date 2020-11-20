<template>
  <form @submit.prevent="create">
    <div>Client: <input v-model="clientUsername" type="text" /></div>
    <div>Title: <input v-model="title" type="text" /></div>
    <div>Description: <input v-model="description" type="text" /></div>
    <nuxt-link to="/">Return</nuxt-link>
    <button type="reset">RESET</button>
    <button @click.prevent="create">CREATE</button>
  </form>
</template>

<script>
export default {
  data(){
      return {
          clientUsername: null,
          designerUsername: this.$auth.user.sub,
          title: null,
          description: null,
      }
  },
  methods: {
     create() {
      this.$axios
        .$post("/api/projects", {
          clientUsername: this.clientUsername,
          designerUsername: this.designerUsername,
          title: this.title,
          description: this.description,
        })
        .then(response => {
          this.$router.push(`/project/${response.id}`);
        });
    }, 
  }
}
</script>

<style scoped>

</style>