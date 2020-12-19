<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <div class="jumbotron">
      <h2>Structure</h2>
    </div>
    <table class="table table-striped">
      <tbody>
      <tr>
        <th scope="row">Name</th>
        <td>{{ structure.name }}</td>
      </tr>
      <tr>
        <th scope="row">Load</th>
        <td>{{ structure.q }}</td>
      </tr>
      <tr>
        <th scope="row">Number</th>
        <td>{{ structure.nb }}</td>
      </tr>
      <tr>
        <th scope="row">Distance</th>
        <td>{{ structure.LVao }}</td>
      </tr>
      <tr>
        <th scope="row">Created</th>
        <td>{{ $moment(structure.created).format("L LT") }}</td>
      </tr>
      <tr v-if="structure.updated">
        <th scope="row">Updated</th>
        <td>{{ $moment(structure.updated).format("L LT") }}</td>
      </tr>
      </tbody>
    </table>
    <nuxt-link type="reset" class="btn btn-primary" :to="`/project/${id}/structure/${idS}/edit`">Edit Structure Details</nuxt-link>
    <p/>
    <div class="jumbotron">
      <h2>Materials TODO Lista materiais</h2>
    </div>
    <nuxt-link class="btn btn-primary" :to="`/project/${this.id}/structure/${this.idS}/simulate`" v-if="$auth.user.groups.includes('Designer')">Add Materials</nuxt-link>
    <p/>
    <table class="table table-striped">
      <tbody>
      <tr>
        <th scope="row">Name</th>
        <td>{{ structure.name }}</td>
      </tr>
      <tr>
        <th scope="row">Load</th>
        <td>{{ structure.q }}</td>
      </tr>
      <tr>
        <th scope="row">Number</th>
        <td>{{ structure.nb }}</td>
      </tr>
      <tr>
        <th scope="row">Distance</th>
        <td>{{ structure.LVao }}</td>
      </tr>
      <tr>
        <th scope="row">Created</th>
        <td>{{ $moment(structure.created).format("L LT") }}</td>
      </tr>
      <tr v-if="structure.updated">
        <th scope="row">Updated</th>
        <td>{{ $moment(structure.updated).format("L LT") }}</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
export default {
  auth: false,
  data() {
    return {
      items: [{
        text: 'Dashboard',
        to: {name: 'dashboard'}
      }, {
        text: 'Project Details',
        to: `/project/${this.$route.params.id}`
      }, {
        text: 'Structure Details',
        active: true
      }],
      structure: {}
    };
  },
  computed: {
    id() {
      return this.$route.params.id
    },
    idS() {
      return this.$route.params.idS
    }
  },
  mounted() {
    //const user = this.username == "me" ? this.$auth.user.sub : this.username
    this.$axios.$get(`/api/projects/${this.id}/structures/${this.idS}`).then((response) => {
      this.structure = response
    })
  },
  methods: {
    create() {
      this.$axios.post(`/api/projects/${this.idProject}/upload`, this.formData(element), {
          headers: {"Content-Type": "multipart/form-data",},
        }
      ).then((response) => {
        if (response.status == 200) {
          this.$toast.success(response.data).goAway(4000)
        } else {
          this.$toast.danger(response.data).goAway(4000)
          ok = false
        }
      });
    },
  },
};
</script>
