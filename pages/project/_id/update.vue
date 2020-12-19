<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <Update/>
  </div>
</template>
<script>
import Update from "@/components/project/create";

export default {
  components: {Update},
  data() {
    return {
      items: [{
        text: 'Dashboard',
        to: {name: 'dashboard'}
      }, {
        text: 'Project Details',
        to: `/project/${this.$route.params.id}`
      }, {
        text: 'Update',
        active: true
      }]
    }
  }
};
</script>
