<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <div class="jumbotron">
      <h2>Product Families</h2>
    </div>
    <nuxt-link class="btn btn-success" :to="{name: 'family-create'}">
      <fa :icon="['fas', 'plus']"/>
    </nuxt-link>
    <b-form-input
      v-model="filter"
      type="search"
      placeholder="Type to Search"
    ></b-form-input>
    <b-table id="table-families" striped hover :items="families" :fields="familiesFields"
             :per-page="perPage" :current-page="currentPage"
             :filter="filter" @filtered="onFiltered">
      <template #cell(actions)="data">
        <nuxt-link class="btn btn-primary" :to="`/family/${data.item.id}`">
          <fa :icon="['fa', 'info-circle']"/>
        </nuxt-link>
      </template>
    </b-table>
    <b-pagination
      v-model="currentPage"
      :total-rows="rows"
      :per-page="perPage"
      limit="10"
      aria-controls="table-families"
    ></b-pagination>
  </div>
</template>
<script>
export default {
  name: "family", // ignored because the name is forced as the folder name
  data: function () {
    return {
      families: [],
      familiesFields: [{
        key: 'id',
        label: 'ID'
      }, {
        key: 'name',
        label: 'Name'
      }, {
        key: 'materialsCount',
        label: 'Num. Materials'
      }, {
        key: 'created',
        label: 'Created Date',
        formatter: (value) => this.$moment(value).format('L LT')
      }, {
        key: 'updated',
        label: 'Updated Date',
        formatter: (value) => value ? this.$moment(value).format('L LT') : ''
      },
        'actions'
      ],
      manufacturerUsername: this.$auth.user.sub,
      items: [{
        text: 'Dashboard',
        to: {name: 'dashboard'}
      }, {
        text: 'Families',
        active: true
      }],
      currentPage: 1,
      rows: 0,
      perPage: 5,
      filter: null
    }
  },
  mounted() {
    this.$axios.$get(`/api/families/`)
      .then((families) => {
        this.families = families;
        this.rows = families.length;
      });
  },
  methods: {
    remove(id) {
      this.$axios.$delete(`/api/families/${id}`).then(r => {
        this.$toast.success('Family deleted with success').goAway(1500)
        this.families = this.families.filter(i => i.id != id);
      }).catch(reason => {
        this.$toast.error('An error occurred while trying to delete.').goAway(1500)
      })
    },
    onFiltered(filteredItems) {
      this.rows = filteredItems.length
      this.currentPage = 1
    }
  }
}
</script>
<style scoped>

</style>
