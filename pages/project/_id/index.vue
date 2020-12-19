<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <div class="jumbotron">
      <h2>Project Details</h2>
    </div>
    <table class="table table-striped">
      <tbody>
      <tr>
        <th scope="row">Client</th>
        <td>{{ project.clientUsername }}</td>
      </tr>
      <tr>
        <th scope="row">Designer</th>
        <td>{{ project.designerUsername }}</td>
      </tr>
      <tr>
        <th scope="row">Title</th>
        <td>{{ project.title }}</td>
      </tr>
      <tr>
        <th scope="row">Description</th>
        <td>{{ project.description }}</td>
      </tr>
      <tr>
        <th scope="row">Created</th>
        <td>{{ $moment(project.created).format("L LT") }}</td>
      </tr>
      <tr v-if="project.updated">
        <th scope="row">Updated</th>
        <td>{{ $moment(project.updated).format("L LT") }}</td>
      </tr>
      </tbody>
    </table>
    <nuxt-link class="btn btn-primary" :to="`/project/${project.id}/update/`">Update Project Details</nuxt-link>
    <nuxt-link class="btn btn-primary" :to="`/project/${project.id}/structure/simulate`" v-if="$auth.user.groups.includes('Designer')">Simulate</nuxt-link>
    <p/>
    <div class="jumbotron" style="padding: 2rem;">
      <h3>Uploads</h3>
    </div>
    <table class="table table-striped">
      <tbody>
        <tr>
          <th>File Name</th>
          <th>Action</th>
        </tr>
        <tr v-for="upload of project.uploadDTOS" :key="upload.id">
          <td>{{upload.filename}}</td>
          <td>
            <button class="btn btn-primary" v-on:click="download(upload.id, upload.filename)"><fa :icon="['fas', 'download']" /></button>
          </td>
        </tr>
      </tbody>
    </table>
    <nuxt-link class="btn btn-primary" :to="`/project/${project.id}/upload`" v-if="$auth.user.groups.includes('Client')">Upload Photos</nuxt-link>
    <p/>
    <div class="jumbotron" style="padding: 2rem;">
      <h3>Structures</h3>
    </div>
    <table class="table table-striped">
      <tbody>
        <tr>
          <th>Name</th>
          <th v-if="$auth.user.groups.includes('Designer')">Visible to Client</th>
          <th>Client Accepted</th>
          <th>Action</th>
        </tr>
        <tr v-for="struct of project.structureDTOS" :key="struct.id">
          <td>{{struct.name}}</td>
          <td v-if="$auth.user.groups.includes('Designer')">{{struct.visibleToClient}}</td>
          <td>{{struct.clientAccepted}}</td>
          <td>
            <nuxt-link class="btn btn-primary" :to="`/project/${project.id}/structure/${struct.id}`"><fa :icon="['fas', 'asterisk']" /></nuxt-link>
            <button v-if="$auth.user.groups.includes('Designer')" class="btn btn-success" v-on:click="showClient(struct.id)"><fa :icon="['fas', 'info']" /></button>
          </td>
        </tr>
      </tbody>
    </table>
    <nuxt-link class="btn btn-primary" :to="`/project/${project.id}/structure/create`" v-if="$auth.user.groups.includes('Designer')">Create Struture</nuxt-link>
  </div>
</template>
<script>
export default {
  name: "project-details",
  data: function () {
    return {
      project: {},
      items: [{
        text: 'Dashboard',
        to: { name: 'dashboard' }
      }, {
        text: 'Project Details',
        active: true
      }]
    }
  },
  computed: {
    id () {
      return this.$route.params.id
    }
  },
  mounted() {
    //const user = this.username == "me" ? this.$auth.user.sub : this.username
    this.$axios.$get(`/api/projects/${this.id}`).then((response) => {
      this.project = response
    })
  },
  methods: {
    download(toId, name){
      this.$axios.$get(`/api/projects/${this.id}/download/${toId}`, { responseType: "arraybuffer" }).then((file) => {
          const url = window.URL.createObjectURL(new Blob([file]));
          const link = document.createElement("a");
          link.href = url;
          link.setAttribute("download", name);
          document.body.appendChild(link);
          link.click();
        });
    },
    trash(toId){
      this.$axios.delete(`/api/projects/${this.id}/delete/${toId}`).then((response) => {
        if(response.status ==202){
          this.$toast.success('File Deleted').goAway(2000)
          location.reload()
        }else {
          this.$toast.error('Something went wrong').goAway(2000)
        }
      })
    },
    showClient(toId){
      this.$axios.patch(`/api/projects/${this.id}/structures/${toId}/visibletoclient`).then((response) => {
        if(response.status == 200){
          this.$toast.success('File Deleted').goAway(2000)
          location.reload()
        }else {
          this.$toast.error('Something went wrong').goAway(2000)
        }
      })
    }
  }
}
</script>

<style scoped>

</style>
