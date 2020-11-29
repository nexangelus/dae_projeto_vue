<template>
  <div>
    <form>
      <div class="form-group">
        <label for="name">Name</label>
        <input v-model="family.name" type="text" class="form-control" id="name">
      </div>
      <button @click.prevent="cancel" class="btn btn-danger">Cancel</button>
      <button @click.prevent="send" class="btn btn-primary">{{this.family && this.family.id ? 'Update' : "Create"}}</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "family-form",
  props: ['family'],
  data() {
    return {
    }
  },
  methods: {
    cancel() {
      this.$router.go(-1)
    },
    send() {
      if(this.family && this.family.id) { // family exists => needs updating
        const family = {name: this.family.name}
        this.$axios.$put(`/api/families/${this.family.id}`, family).then((f) => {
          this.$router.push(`/family/${f.id}`)
        });
      } else { // create a new family
        const family = {name: this.family.name, manufacturerUsername: this.$auth.user.sub}
        this.$axios.$post(`/api/families/`, family).then((f) => {
          this.$router.push(`/family/${f.id}`)
        });
      }
    }
  }
}
</script>

<style scoped>

</style>
