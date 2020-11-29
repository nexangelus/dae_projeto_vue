<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <div class="jumbotron">
      <h2>Simulate</h2>
    </div>
    <form @submit.prevent="create">
    
    
        <b-button type="submit" :disabled="!hasFile">SAVE</b-button>
    </form>
  </div>
</template>
<script>
export default {
  auth: false,
  data() {
    return {
      idProject: this.$route.params.id,
      items: [{
        text: 'Dashboard',
        to: { name: 'dashboard' }
      }, {
        text: 'Project',
        to: `/project/${this.$route.params.id}`
      }, {
        text: 'Simulate',
        active: true
      }]
    };
  },
  computed: {
    hasFile() {
      return this.file != null;
    },
  },
  methods: {
    create() {
      this.$axios.post(`/api/projects/${this.idProject}/upload`, this.formData(element),{
            headers: {"Content-Type": "multipart/form-data",},
          }
        ).then((response) =>{
          if(response.status ==200){
            this.$toast.success(response.data).goAway(4000)
          }else{
            this.$toast.danger(response.data).goAway(4000)
            ok = false
          }
        });
    },
  },
};
</script>