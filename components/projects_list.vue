<template>
  <div>
    <b-form-input
      v-model="filter"
      type="search"
      placeholder="Type to Search"
    ></b-form-input>
    <b-table striped hover :items="projects" :fields="projectFields"
             :per-page="perPage" :current-page="currentPage"
             :filter="filter">
      <template #cell(actions)="data">
        <nuxt-link class="btn btn-primary" :to="`/project/${data.item.id}`"><fa :icon="['fas', 'info']" /></nuxt-link>
      </template>
    </b-table>
    <b-pagination
      v-model="currentPage"
      :total-rows="rows"
      :per-page="perPage"
      limit="10"
      aria-controls="table-profiles"
    ></b-pagination>
  </div>
</template>
<script>
export default {
  props: ['projects'],
  data() {
    return {
      filter: null,
      currentPage: 1,
      perPage: 5,
      projectFields: [
        {
          key: 'title',
          label: 'Title'
        }, {
          key: 'description',
          label: 'Description'
        }, {
          key: 'clientUsername',
          label: 'Client'
        }, {
          key: 'designerUsername',
          label: 'Designer'
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
      ]
    }
  },
  computed: {
    rows() {
      return this.projects ? this.projects.length : 0
    }
  }
}
</script>

<style scoped>

</style>
