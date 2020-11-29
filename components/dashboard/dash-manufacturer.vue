<template>
  <div class="container-fluid">
    <h1>Olá {{manufacturer.name}}</h1>

  </div>
</template>

<script>
export default {
  name: "dash-manufacturer",
  data: function () {
    return {
      families: [],
      materials: [],
      manufacturer: {},
    }
  },
  computed: {
    username () {
      return this.$auth.user.sub
    }
  }, // TODO incluir uma lista com os materias que foram utilizados recentemente numa estrutura ???
  mounted() {
    //const user = this.username == "me" ? this.$auth.user.sub : this.username
    this.$axios.$get(`/api/manufacturers/${this.username}`).then((response) => {
      this.manufacturer = response;
    })
  }
}
</script>

<style scoped>

</style>
