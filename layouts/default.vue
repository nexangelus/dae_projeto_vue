<template>
  <div>
    <b-navbar toggleable="lg" type="dark" variant="info">
      <b-navbar-brand :to="{name:'dashboard'}">DAE</b-navbar-brand>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav>
          <b-nav-item-dropdown text="Project" v-if="$auth.user && $auth.user.groups.includes('Designer')">
            <b-dropdown-item :to="{name: 'project-create'}">Create</b-dropdown-item>
          </b-nav-item-dropdown>
          <b-nav-item-dropdown text="Products Families" v-if="$auth.user && $auth.user.groups.includes('Manufacturer')">
            <b-dropdown-item :to="{name: 'family'}">List</b-dropdown-item>
            <b-dropdown-item :to="{name: 'family-create'}">Create</b-dropdown-item>
          </b-nav-item-dropdown>
          <b-nav-item-dropdown text="Account" v-if="$auth.user && $auth.user.groups.includes('Designer')">
            <b-dropdown-item :to="{name: 'accounts-create'}">Create</b-dropdown-item>
          </b-nav-item-dropdown>
          <b-nav-item-dropdown text="Materials" v-if="$auth.user && $auth.user.groups.includes('Manufacturer')">
            <b-dropdown-item :to="{name: 'materials'}">List</b-dropdown-item>
            <b-dropdown-item :to="{name: 'materials-create'}">Create</b-dropdown-item>
          </b-nav-item-dropdown>
        </b-navbar-nav>

        <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">
          <b-nav-item-dropdown right>
            <!-- Using 'button-content' slot -->
            <template #button-content v-if="$auth.user"> {{$auth.user.sub}} </template>
            <b-dropdown-item v-if="$auth.user" :to="`/${group}/${$auth.user.sub}/`">Profile</b-dropdown-item>
            <b-dropdown-item @click.prevent="signOut">Sign Out</b-dropdown-item>
          </b-nav-item-dropdown>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
    <div id="main">
      <Nuxt />
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      group: this.$auth.user ? this.$auth.user.groups[0].toLowerCase() : ""
    }
  },
  methods: {
    signOut() {
      this.$auth.logout();
      this.$router.push("/");
    },
  },
};
</script>

<style>
html {
  font-family: "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI",
    Roboto, "Helvetica Neue", Arial, sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
}
/*#main {
  background-color: #313334;
  color: white;
}*/

#main {
  margin-top: 15px;
  margin-bottom: 15px;
}

.button--green {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #3b8070;
  color: #3b8070;
  text-decoration: none;
  padding: 10px 30px;
}

.button--green:hover {
  color: #fff;
  background-color: #3b8070;
}

.button--grey {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #35495e;
  color: #35495e;
  text-decoration: none;
  padding: 10px 30px;
  margin-left: 15px;
}

.button--grey:hover {
  color: #fff;
  background-color: #35495e;
}
</style>
