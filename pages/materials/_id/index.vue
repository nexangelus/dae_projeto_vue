<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <div class="jumbotron">
      <h2>Material Details</h2>
    </div>
    <table class="table table-striped">
      <tbody>
      <tr>
        <th scope="row">Name</th>
        <td>{{ material.name }}</td>
      </tr>
      <tr>
        <th scope="row">Family</th>
        <td>{{ material.family.name }}</td>
      </tr>

      <template v-if="material.profile">
        <tr>
          <th scope="row">Weff Positive</th>
          <td>{{ material.profile.weff_p }}</td>
        </tr>
        <tr>
          <th scope="row">Weff Negative</th>
          <td>{{ material.profile.weff_p }}</td>
        </tr>
        <tr>
          <th scope="row">Ar</th>
          <td>{{ material.profile.ar }}</td>
        </tr>
        <tr>
          <th scope="row">Sigma C</th>
          <td>{{ material.profile.sigmaC }}</td>
        </tr>
        <tr>
          <th scope="row">PP</th>
          <td>{{ material.profile.pp }}</td>
        </tr>
        <tr>
          <th scope="row">MCR Positive</th>
          <td class="other-table"><mcr_view_table :val="material.profile.mcr_p" /></td>
        </tr>
        <tr>
          <th scope="row">MCR Negative</th>
          <td class="other-table"><mcr_view_table :val="material.profile.mcr_n" /></td>
        </tr>
      </template>

      <template v-else-if="material.sheet">
        <tr>
          <th scope="row">Thickness</th>
          <td>{{ material.sheet.thickness }}</td>
        </tr>
      </template>

      <tr>
        <th scope="row">Created</th>
        <td>{{ $moment(material.created).format("L LT") }}</td>
      </tr>
      <tr v-if="material.updated">
        <th scope="row">Updated</th>
        <td>{{ $moment(material.updated).format("L LT") }}</td>
      </tr>
      </tbody>
    </table>
    <nuxt-link class="btn btn-primary" v-if="material.manufacturerUsername === this.$auth.user.sub" :to="`/materials/${material.id}/update`">Update Material</nuxt-link>
  </div>
</template>

<script>
import Mcr_view_table from "@/components/material/mcr_view_table";
export default {
  components: {Mcr_view_table},
  data() {
    return {
      material: {family: {}},
      items: [{
        text: 'Dashboard',
        to: { name: 'dashboard' }
      }, {
        text: 'Materials',
        to: { name: 'materials' }
      }, {
        text: `${this.$route.params.id}`,
        active: true
      }]
    };
  },
  computed: {
    id () {
      return this.$route.params.id
    }
  },
  created() {
    this.$axios.$get(`/api/materials/${this.id}`).then((family) => {
      this.material = family;
    });
  },
};
</script>

<style scoped>
.other-table {
  padding: 0;
  border-top: 0;
}
</style>
