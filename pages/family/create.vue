<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <family-form :family="{name:''}"></family-form>
  </div>
</template>

<script>
import FamilyForm from "@/components/family/form";
export default {
  name: "family-create",
  components: {FamilyForm},
  data() {
    return {
      items: [{
        text: 'Dashboard',
        to: { name: 'dashboard' }
      }, {
        text: 'Families',
        to: { name: 'family' }
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
