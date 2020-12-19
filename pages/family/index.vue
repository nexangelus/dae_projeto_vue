<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <div class="jumbotron">
      <h2>Product Families</h2>
    </div>
    <nuxt-link class="btn btn-success" :to="{name: 'family-create'}"><fa :icon="['fas', 'plus']" /></nuxt-link>
    <table class="table table-striped"> TODO TABLE
      <tr>
        <th>ID</th>
        <th>Name</th>
        <th>Num. Materials</th>
        <th>Created Date</th>
        <th>Updated Date</th>
        <th>Actions</th>
      </tr>
      <tr v-for="family of families" :key="family.id">
        <td>{{family.id}}</td>
        <td>{{family.name}}</td>
        <td>{{family.materialsCount}}</td>
        <td>{{$moment(family.created).format('L LT')}}</td>
        <td>{{family.updated ? $moment(family.updated).format("L LT") : ''}}</td>
        <td>
          <nuxt-link class="btn btn-primary" :to="`/family/${family.id}`"><fa :icon="['fa', 'info-circle']" /></nuxt-link>
        </td>
      </tr>
    </table>
  </div>
</template>
<script>
export default {
  name: "family", // ignored because the name is forced as the folder name
  data: function () {
    return {
      families: [],
      manufacturerUsername: this.$auth.user.sub,
      items: [{
        text: 'Dashboard',
        to: { name: 'dashboard' }
      }, {
        text: 'Families',
        active: true
      }]
    }
  },
  mounted() {
    this.$axios.$get(`/api/families/`)
      .then((families) => {
        this.families = families;
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
    }
  }
}
</script>
<style scoped>

</style>
