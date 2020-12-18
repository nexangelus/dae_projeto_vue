<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <material-form></material-form>
  </div>
</template>

<script>
import MaterialForm from "@/components/material/form";
export default {
  name: "materials-create",
  components: {MaterialForm},
  data() {
    return {
      items: [{
        text: 'Dashboard',
        to: { name: 'dashboard' }
      }, {
        text: 'Materials',
        to: { name: 'materials' }
      }, {
        text: 'Create',
        active: true
      }]
    }
  }
}
</script>

<style scoped>

</style>
