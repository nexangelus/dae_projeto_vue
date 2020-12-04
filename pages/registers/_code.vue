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
                  disabled
                />
              </div>
              <div class="form-group">
                <label>Name</label>
                <input
                  v-model="name"
                  type="text"
                  class="form-control"
                  placeholder="Name"
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
              <div class="form-group" v-if="group ==='Client'">
                <label>Address</label>
                <input
                  v-model="address"
                  type="text"
                  class="form-control"
                  placeholder="Address"
                />
              </div>
              <div class="form-group" v-if="group ==='Client'">
                <label>Contact</label>
                <input
                  v-model="contact"
                  type="text"
                  class="form-control"
                  placeholder="Contact"
                />
              </div>
              //TODO resto dos utilizadores
              <button @click.prevent="register" class="btn btn-secondary">Register</button>
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
      name: null,
      email: null,
      contact: null,
      address: null,
      code: this.$route.params.code,
      group: null
    };
  },
  mounted() {
    this.$axios.$get(`/api/accounts/${this.code}`).then((response) => {
      console.log(response)
      this.group = response.groupType;
      this.email = response.email;
    })
  },
  methods: {
    register(){
      this.$axios.$post(`/api/${this.group.toLowerCase()}s/${this.code}`, {
        username: this.username,
        email: this.email,
        newPassword: this.password,
        contact: this.contact,
        address: this.address,
        name: this.name
      })
      .then((response) => {
        if(response.code == 200){
          this.$toast.success("User created")
        }else{
          this.$toast.danger("Error")
        }
          

      })
    }
  },
};
</script>

<style>

</style>