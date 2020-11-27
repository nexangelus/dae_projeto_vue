<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <div class="jumbotron">
      <h2>Upload Files</h2>
    </div>
    <form @submit.prevent="upload">
    <b-form-file
      multiple
      v-model="file"
      :state="hasFile"
      placeholder="Choose a file or drop it here..."
      drop-placeholder="Drop file here..."
    ></b-form-file>
    <div class="mt-3" v-for="fil of file" :key="key">
      Selected file: {{ fil ? fil.name : "" }}
    </div>
    <b-button type="submit" :disabled="!hasFile">Upload</b-button>
  </form>
  </div>
</template>
<script>
export default {
  auth: false,
  data() {
    return {
      file: null,
      idProject: this.$route.params.id,
      items: [{
        text: 'Dashboard',
        to: { name: 'dashboard' }
      }, {
        text: 'Project Details',
        to: `/project/${this.$route.params.id}`
      }, {
        text: 'Upload Files',
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
    formData(ele){
      let formData = new FormData();
      formData.append("file", ele)
      return formData
    },
    upload() {
      let ok = true
      if (!this.hasFile) {
        return;
      }
      this.file.forEach((element) => {
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
      });
      this.$router.push(`/project/${this.$route.params.id}`)
    },
  },
};
</script>
