<template>
  <b-container>
    <h4>Profile Details</h4>
    <p>Name: {{ user.name }}</p>
    <p>Email: {{ user.email }}</p>
    <p>Created: {{ user.created }}</p>
    <p v-if="user.updated">Updated: {{ user.updated }}</p>
    <nuxt-link :to="`/${$auth.user.groups[0].toLowerCase()}/${$auth.user.sub}/update/`">
      UPDATE
    </nuxt-link>
  </b-container>
</template>
<script>
export default {
  data() {
    return {
      user: [],
      type: this.$auth.user.groups.includes("client"),
    };
  },
  mounted() {
    this.$axios
      .$get(
        `/api/${this.$auth.user.groups[0].toLowerCase()}s/${
          this.$auth.user.sub
        }`
      )
      .then((user) => {
        this.user = user;
      });
  },
};
</script>