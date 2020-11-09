<template>
  <div class="container">
    <div>
      <Logo />
      <h1 class="title">projeto</h1>
      <div class="container" style="min-height: 0">
        <div class="col-md-12 col-sm-12">
          <div class="login-form">
            <form>
              <div class="form-group">
                <label>User Name</label>
                <input
                  v-model="username"
                  type="text"
                  class="form-control"
                  placeholder="User Name"
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
              <button @click.prevent="login" class="btn btn-secondary">Login</button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      username: null,
      password: null,
    };
  },
  methods: {
    login() {
      let credentials = `username=${this.username}&password=${this.password}`

      this.$axios
        .$post("/api/login/token", credentials)
        .then((response) => {
          
        });
    },
  },
};
</script>

<style>
*{
  background-color: #313334;
  color: white;
}
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
