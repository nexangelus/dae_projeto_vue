<template>
  <div class="container">
    <div>
      <Logo />
      <h1 class="title">projeto</h1>
      <div class="container-fluid" style="min-height: 0">
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
          <br>
          <p><a class="btn btn-outline-secondary btn-sm" @click="log('admin', 'admin')"> LOGIN: Admin</a></p>
          <p>
            <a class="btn btn-outline-secondary btn-sm" @click="log('manu', 'manu')"> LOGIN: Manufacturer 1</a>
            <a class="btn btn-outline-secondary btn-sm" @click="log('fernando78', 'EO8qP4ATEA')"> LOGIN: Manufacturer 2</a>
          </p>
          <p>
            <a class="btn btn-outline-secondary btn-sm" @click="log('client', 'client')"> LOGIN: Client 1</a>
            <a class="btn btn-outline-secondary btn-sm" @click="log('neves.nelson', 'XJQAioWQjV')"> LOGIN: Client 2</a>
          </p>
          <p>
            <a class="btn btn-outline-secondary btn-sm" @click="log('designer', 'designer')"> LOGIN: Designer 1</a>
            <a class="btn btn-outline-secondary btn-sm" @click="log('bpinto', 'icLMoqJM99')"> LOGIN: Designer 2</a>
          </p>
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
      const promise = this.$auth.loginWith('local', {
        data: {
          username: this.username,
          password: this.password
        }
      })
      promise.then(() => {
        this.$toast.success('You are logged in!').goAway(1500)
        // Admin, Client, Designer, Manufacturer
        const groups = this.$auth.user.groups;
        const username = this.$auth.user.sub;
        if(groups.length >= 1) {
          this.$router.push("/dashboard");
        }
        /*if (groups.includes('Admin')) {
          this.$router.push('/admin')
        } else if (groups.includes('Client')) {
          this.$router.push(`/client/${username}`)
        } else if (groups.includes('Designer')) {
          this.$router.push(`/designer/${username}`)
        } else if (groups.includes('Manufacturer')) {
          this.$router.push(`/manufacturer/${username}`)
        }*/
      })
      promise.catch(() => {
        this.$toast.error('Sorry, you cannot login. Ensure your credentials are correct').goAway(1500)
      })
    },
    log(username, password) {
      this.username = username;
      this.password = password;
      this.login();
    }
  },
};
</script>

<style>
/**{
  background-color: #313334;
  color: white;
}*/
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
