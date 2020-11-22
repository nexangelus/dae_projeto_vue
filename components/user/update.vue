<template>
  <form @submit.prevent="create">
    <div>Password: <input v-model="user.password" type="password" /></div>
    <div>Name: <input v-model="user.name" type="text" /></div>
    <div>Email: <input v-model="user.email" type="email" /></div>
    <div v-if="$auth.user.groups.includes('client')">
        <div>Address <input v-model="user.address" type="text"/></div>
        <div>Contact <input v-model="user.contact" type="text"/></div>
    </div>
    
    <nuxt-link to="/">Return</nuxt-link>
    <button type="reset">RESET</button>
    <button @click.prevent="update">UPDATE</button>
  </form>
</template>
<script>
export default {
  data() {
    return {
      user: [],
      type: this.$auth.user.groups.includes('client')
    };
  },
  mounted() {
    this.$axios.$get(`/api/${this.$auth.user.groups[0].toLowerCase()}s/${this.$auth.user.sub}`).then((user) => {
      this.user = user
    });
  },
  methods: {
    update() {
      this.$axios
        .$put(`/api/${this.$auth.user.groups[0].toLowerCase()}s/${this.$auth.user.sub}`, {
          username: this.user.username,
          password: this.user.password,
          name: this.user.name,
          email: this.user.email
        })
        .then(() => {
            //TODO voltar para o profile
          this.$router.push(`/${$auth.user.groups[0].toLowerCase()}/${$auth.user.sub}/`);
        });
    },
  },
};
</script>