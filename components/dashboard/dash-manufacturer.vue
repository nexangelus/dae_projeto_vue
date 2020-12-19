<template>
  <div class="container-fluid">
    <h1>Olá {{ manufacturer.name }}</h1>
    <p></p>
    <div class="jumbotron">
      <h2>Structures with my materials</h2>
    </div>
    <b-form-input
      v-model="filter"
      type="search"
      placeholder="Type to Search"
    ></b-form-input>
    <b-table id="table-structures" striped hover :items="structures" :fields="structuresFields" :per-page="perPage"
             :current-page="currentPage" :filter="filter" @filtered="onFiltered">
    </b-table>
    <b-pagination
      v-model="currentPage"
      :total-rows="rows"
      :per-page="perPage"
      limit="10"
      aria-controls="table-structures"
    ></b-pagination>
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
      currentPage: 1,
      rows: 0,
      perPage: 5,
      filter: null
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
  },
  methods: {
    onFiltered(filteredItems) {
      this.rows = filteredItems.length
      this.currentPage = 1
    }
  }
}
</script>

<style scoped>

</style>
