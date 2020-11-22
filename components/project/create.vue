<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <div class="jumbotron">
      <h2>Create new Project</h2>
    </div>
    <form @submit.prevent="create">
      <div class="form-group">
        <label for="clientUsername">Client</label>
        <input id="clientUsername" class="form-control" v-model="clientUsername" type="text"/></div>
      <div class="form-group">
        <label for="title">Project Title</label>
        <input v-model="title" id="title" class="form-control" type="text"/></div>
      <div class="form-group">
        <label for="description">Project Description</label>
        <textarea class="form-control" id="description" v-model="description" rows="3"></textarea>
      </div>
      <button type="reset" class="btn btn-danger">Clear</button>
      <button @click.prevent="create" class="btn btn-primary">Create</button>
    </form>
  </div>

</template>

<script>
export default {
  data() {
    return {
      clientUsername: null,
      designerUsername: this.$auth.user.sub,
      title: null,
      description: null,
      items: [{
        text: 'Projects',
        href: '#'
      }, {
        text: 'Create',
        active: true
      }]
    }
  },
  methods: {
    create() {
      this.$axios
        .$post("/api/projects", {
          clientUsername: this.clientUsername,
          designerUsername: this.designerUsername,
          title: this.title,
          description: this.description,
        })
        .then(response => {
          this.$router.push(`/project/${response.id}`);
        });
    },
  }
}
</script>

<style scoped>

</style>
