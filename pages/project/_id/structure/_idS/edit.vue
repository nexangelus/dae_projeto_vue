<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <div class="jumbotron">
      <h2>Edit Struture</h2>
    </div>
    <form>
      <div class="form-group">
        <label for="name">Name</label>
        <input id="name" class="form-control" v-model="structure.name">
      </div>
      <div class="form-group">
        <label for="nb">Number</label>
        <input id="nb" class="form-control" v-model="structure.nb">
      </div>
      <div class="form-group">
        <label for="LVao">Distance</label>
        <input id="LVao" class="form-control" v-model="structure.LVao">
      </div>
      <div class="form-group">
        <label for="q">Load</label>
        <input id="q" class="form-control" v-model="structure.q">
      </div>
      <button type="reset" class="btn btn-danger">Clear</button>
      <button @click.prevent="update" class="btn btn-primary">Update</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "edit",
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
        to: `/project/${this.$route.params.id}/structure/${this.$route.params.idS}`
      }, {
        text: 'Edit',
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
  created() {
    this.$axios.$get(`/api/projects/${this.id}/structures/${this.idS}`).then((response) => {
      this.structure = response
    })
  },
  methods: {
    update() {
      this.$axios.put(`/api/projects/${this.id}/structures/${this.idS}`, this.structure).then((response) => {
        if (response.status === 200) {
          this.$toast.success("Updated").goAway(4000)
          this.$router.push(`/project/${this.$route.params.id}/structure/${this.idS}`)
        } else {
          this.$toast.error("Something went wrong").goAway(4000)
        }
      }).catch(r => {
        this.$toast.error("Something went wrong").goAway(4000)
      });
    },
  },
}
</script>

<style scoped>

</style>
