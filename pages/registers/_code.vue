<template>
  <div class="container">
    <div>
      <h1 class="title">Register</h1>
      <div class="container-fluid" style="min-height: 0">
        <div class="col-md-12 col-sm-12">
          <div class="login-form">
            <form>
              <div class="form-group">
                <label>Username</label>
                <input
                  v-model="username"
                  type="text"
                  class="form-control"
                  placeholder="Username"
                />
              </div>
              <div class="form-group">
                <label>Email</label>
                <input
                  v-model="email"
                  type="email"
                  class="form-control"
                  placeholder="Email"
                />
              </div>
              <div class="form-group">
                <label>Password</label>
                <input
                  v-model="password"
                  type="password"
                  class="form-control"
                  placeholder="Password"
                />
              </div>
              //TODO resto dos utilizadores
              <button @click.prevent="register" class="btn btn-secondary">Login</button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  auth: "guest",
  data() {
    return {
      username: null,
      password: null,
      email: null,
      code: this.$route.params.code,
      group: null
    };
  },
  mounted() {
    this.$axios.$get(`/api/accounts/${this.code}`).then((response) => {
      this.group = response.group;
      this.email = response.email;
    })
  },
  methods: {
    register(){

    }
  },
};
</script>

<style>

</style>