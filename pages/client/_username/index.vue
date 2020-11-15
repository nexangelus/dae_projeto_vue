<template>
  <div>
    <h1>Olá {{client.name}}</h1>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      client: {name:""},
    }
  },
  computed: {
    username () {
      return this.$route.params.username
    }
  },
  mounted() {
    //const user = this.username == "me" ? this.$auth.user.sub : this.username
    this.$axios.$get(`/api/clients/${this.username}`).then((response) => {
      this.client = response;
    })
  }
}
</script>

<style scoped>

</style>
