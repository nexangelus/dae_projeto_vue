<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>
    <family-form v-if="family" :family="family"></family-form>
  </div>
</template>

<script>
import FamilyForm from "@/components/family/form";

export default {
  name: "update",
  components: {FamilyForm},
  data() {
    return {
      family: {},
      items: [{
        text: 'Dashboard',
        to: {name: 'dashboard'}
      }, {
        text: 'Families',
        to: {name: 'family'}
      }, {
        text: `${this.$route.params.id}`,
        to: `/family/${this.$route.params.id}`,
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
    this.$axios.$get(`/api/families/${this.id}`).then(f => {
      this.family = f;
    })
  }
}
</script>

<style scoped>

</style>
