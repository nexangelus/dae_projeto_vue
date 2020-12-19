<template>
  <div class="container-fluid">
    <h1>Olá {{ manufacturer.name }}</h1>
    <p></p>
    <div class="jumbotron">
      <h2>Structures with my materials</h2>
    </div>
    <b-table striped hover :items="structures" :fields="structuresFields"> TODO TABLE

    </b-table>
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
      structures: [],
      structuresFields: [
        {
          key: 'name',
          label: 'Name'
        }, {
          key: 'materials[0].name',
          label: 'Material Name'
        }, {
          key: 'created',
          label: 'Created Date',
          formatter: (value) => this.$moment(value).format('L LT')
        }, {
          key: 'updated',
          label: 'Updated Date',
          formatter: (value) => value ? this.$moment(value).format('L LT') : ''
        }
      ],
    }
  },
  computed: {
    username() {
      return this.$auth.user.sub
    }
  }, // TODO incluir uma lista com os materias que foram utilizados recentemente numa estrutura ???
  mounted() {
    //const user = this.username == "me" ? this.$auth.user.sub : this.username
    this.$axios.$get(`/api/manufacturers/${this.username}`).then((response) => {
      this.manufacturer = response;
    })

    this.$axios.$get('/api/manufacturers/structures').then(r => {
      this.structures = r;
    })
  }
}
</script>

<style scoped>

</style>
