<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <material-form :material="material"></material-form>
  </div>
</template>

<script>
import FamilyForm from "@/components/family/form";
import MaterialForm from "@/components/material/form";

export default {
  name: "update",
  components: {MaterialForm, FamilyForm},
  data() {
    return {
      material: {profile: {}, sheet: {}, family: {}},
      items: [{
        text: 'Dashboard',
        to: {name: 'dashboard'}
      }, {
        text: 'Materials',
        to: {name: 'materials'}
      }, {
        text: `${this.$route.params.id}`,
        to: `/materials/${this.$route.params.id}`,
      }, {
        text: 'Update',
        active: true,
      }]
    }
  },
  computed: {
    id() {
      return this.$route.params.id
    }
  },
  created() {
    this.$axios.$get(`/api/materials/${this.id}`).then(m => {
      this.material = m;
      if(m.profile) {
        this.material.type = "Profile"
      } else if (m.sheet) {
        this.material.type = "Sheet"
      }
    })
  }
}
</script>

<style scoped>

</style>
