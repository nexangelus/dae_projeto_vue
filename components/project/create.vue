<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <div class="jumbotron">
      <h2>Create new Project</h2>
    </div>
    <form>
      <div v-if="!this.isUpdate" class="form-group">
        <label for="clientUsername">Client</label>
        <input id="clientUsername" class="form-control" list="clients" v-model="clientSearch" @keyup="searchClients">
        <datalist id="clients">
          <option v-for="client of clientsSearch" :value="client.name">{{ client.username }}</option>
        </datalist>
      </div>
      <div class="form-group">
        <label for="title">Project Title</label>
        <input v-model="title" id="title" class="form-control" type="text"/>
      </div>
      <div class="form-group">
        <label for="description">Project Description</label>
        <textarea class="form-control" id="description" v-model="description" rows="3"></textarea>
      </div>
      <button type="reset" class="btn btn-danger">Clear</button>
      <button @click.prevent="choise" class="btn btn-primary">
        <p v-if="!this.isUpdate" style="margin-bottom: 0rem;">Create</p>
        <p v-else style="margin-bottom: 0rem;">Update</p>
        </button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      clientSearch: null,
      clientsSearch: [],
      clientName: null,
      designerUsername: this.$auth.user.sub,
      title: null,
      description: null,
      items: [{
        text: 'Dashboard',
        to: { name: 'dashboard' }
      }, {
        text: 'Project',
        to: { name: 'project' }
      }, {
        text: 'Create',
        to: { name: 'project-create' }
      }]
    }
  },
  computed: {
    id () {
      return this.$route.params.id
    },
    isUpdate(){
      return (this.$route.name == "project-id-update")? true : false 
    }
  },
  mounted() {
    if(this.isUpdate){
        this.$axios.$get(`/api/projects/${this.id}`).then((response) => {
          this.title = response.title
          this.description = response.description
          this.clientSearch = response.clientUsername
      })
    }
  },
  methods: {
    choise(){
      if(this.isUpdate){
        this.update()
      }else{
        this.create()
      }
    },
    create() {
      let username = this.clientsSearch.find(x => x.name == this.clientSearch)
      if(username) {
        this.$axios.$post("/api/projects", {
          clientUsername: username.username,
          designerUsername: this.designerUsername,
          title: this.title,
          description: this.description,
        })
        .then(response => {
          this.$router.push(`/project/${response.id}`);
        });
      }

    },
    update() {
      let username = this.clientsSearch.find(x => x.name == this.clientSearch)     
        this.$axios.$put(`/api/projects/${this.id}`, {
          designerUsername: this.designerUsername,
          title: this.title,
          description: this.description,
        })
        .then(response => {
          this.$router.push(`/project/${this.id}`);
        });
    },
    searchClients() {
      if(this.clientSearch.length > 0) {
        this.$axios.$get(`/api/clients/search/${this.clientSearch}`).then(r => {
          this.clientsSearch = r;
        })
      } else {
        this.clientsSearch = [];
      }
    }
  }
}
</script>

<style scoped>

</style>
