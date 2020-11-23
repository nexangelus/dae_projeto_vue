<template>
  <div class="container-fluid">
    <b-breadcrumb :items="items"></b-breadcrumb>

    <dash-client v-if="userType === 'client'"></dash-client>
    <dash-admin v-if="userType === 'admin'"></dash-admin>
    <dash-designer v-if="userType === 'designer'"></dash-designer>
    <dash-manufacturer v-if="userType === 'manufacturer'"></dash-manufacturer>
  </div>
</template>

<script>
import DashClient from "@/components/dashboard/dash-client";
import DashManufacturer from "@/components/dashboard/dash-manufacturer";
import DashAdmin from "@/components/dashboard/dash-admin";
import DashDesigner from "@/components/dashboard/dash-designer";

export default {
  name: "dashboard",
  components: {DashDesigner, DashAdmin, DashManufacturer, DashClient},
  data() {
    return {
      userType: this.$auth.user.groups ? this.$auth.user.groups[0].toLowerCase() : "",
      items: [{
        text: 'Dashboard',
        to: {name: 'dashboard'}
      }]
    }
  }
}
</script>

<style scoped>

</style>
