<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <div class="jumbotron">
      <h2>Create Struture</h2>
    </div>
    <form>
      <div  class="form-group">
        <label for="name">Name</label>
        <input id="name" class="form-control" v-model="structure.name">
      </div>
      <div  class="form-group">
        <label for="nb">Number</label>
        <input id="nb" class="form-control" v-model="structure.nb" type="number">
      </div>
      <div  class="form-group">
        <label for="LVao">Distance</label>
        <input id="LVao" class="form-control" v-model="structure.LVao" type="number">
      </div>
      <div  class="form-group">
        <label for="q">Load</label>
        <input id="q" class="form-control" v-model="structure.q" type="number">
      </div>
      <button type="reset" class="btn btn-danger">Clear</button>
      <button @click.prevent="create" class="btn btn-primary">Create</button>
    </form>
  </div>
</template>
<script>
export default {
  auth: false,
  data() {
    return {
      idProject: this.$route.params.id,
      structure: {},
      items: [{
        text: 'Dashboard',
        to: { name: 'dashboard' }
      }, {
        text: 'Project Details',
        to: `/project/${this.$route.params.id}`
      }, {
        text: 'Create Structure',
        active: false
      }]
    };
  },
  methods: {
    create() {
      this.$axios.post(`/api/projects/${this.idProject}/structures`, this.structure
        ).then((response) =>{
          if(response.status ==201){
            this.$toast.success("Created").goAway(4000)
            this.$router.push(`/project/${this.$route.params.id}`)
          }else{
            this.$toast.error("Something went wrong").goAway(4000)
          }
        });
    },
  },
};
</script>
