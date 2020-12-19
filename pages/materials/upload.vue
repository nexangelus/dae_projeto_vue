<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <div class="jumbotron">
      <h2>Upload Files</h2>
    </div>
    <div class="alert alert-secondary" role="alert">
      <h4 class="alert-heading">Warning!</h4>
      <p>You can download an empty file <a href="/empty.xlsx">here</a>, and an example file <a href="/example.xlsx">here</a>.</p>
      <hr>
      <p class="mb-0">This process must only be done when you know that the file is correct. If you select a wrong file and proceed with the upload, it can mess up your list of materials.</p>
      <p class="mb-0">You must make sure that the file name is like the following:</p>
      <pre>{Family Name}.xlsx</pre>
      <p class="mb-0">Where {Family Name} is the name of the Family of Materials you're importing, example: </p>
      <pre>Section C S220 BF.xlsx</pre>
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
  name: "materials-upload",
  data() {
    return {
      file: null,
      idManu: this.$route.params.id,
      items: [{
        text: 'Dashboard',
        to: { name: 'dashboard' }
      }, {
        text: 'Materials',
        to: { name: 'materials' }
      }, {
        text: 'Import',
        active: true
      }]
    };
  },
  computed: {
    hasFile() {
      return this.file != null;
    },
    username () {
      return this.$auth.user.sub
    }
  },
  methods: {
    formData(ele){
      let formData = new FormData();
      formData.append("file", ele)
      return formData
    },
    upload() {
      let ok = true
      let count = this.file.length
      let starter = 0
      if (!this.hasFile) {
        return;
      }
      this.file.forEach((element) => {
        this.$axios.post(`/api/manufacturers/upload`, this.formData(element),{
            headers: {"Content-Type": "multipart/form-data"},
          }
        ).then((response) =>{
          if(response.status == 200){
            this.$toast.success(response.data).goAway(4000)
          }else{
            this.$toast.danger(response.data).goAway(4000)
            ok = false
          }
          starter++
          if(count == starter && ok){
            this.$router.push('/materials/')
          }
        });
      });
    },
  },
};
</script>
