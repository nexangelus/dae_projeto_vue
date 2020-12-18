<template>
  <div>
    <form>
      <div class="form-group">
        <label for="family">Family</label>
        <b-form-select v-model="material.family.id" :options="families" id="family" class="form-control"></b-form-select>
      </div>
      <div class="form-group">
        <label for="name">Name</label>
        <input v-model="material.name" type="text" class="form-control" id="name">
      </div>
      <div class="form-group">
        <label for="type">Type</label>
        <b-form-select v-model="material.type" :options="types" id="type" class="form-control"></b-form-select>
      </div>
      <div v-if="material.type === 'Profile'" class="group">
        <h6>Profile</h6>
        <div class="form-group">
          <label for="weff_p">Weff P</label>
          <input v-model="material.profile.weff_p" type="text" class="form-control" id="weff_p">
        </div>
        <div class="form-group">
          <label for="weff_n">Weff N</label>
          <input v-model="material.profile.weff_n" type="text" class="form-control" id="weff_n">
        </div>
        <div class="form-group">
          <label for="ar">AR</label>
          <input v-model="material.profile.ar" type="text" class="form-control" id="ar">
        </div>
        <div class="form-group">
          <label for="sigmaC">Sigma C</label>
          <input v-model="material.profile.sigmaC" type="text" class="form-control" id="sigmaC">
        </div>
        <div class="form-group" v-if="material.profile">
          <div class="row">
            <mcr_table :type-short="'P'" :mcr="material.profile.mcr_p" ref="mcr_p"></mcr_table>
            <mcr_table :type-short="'N'" :mcr="material.profile.mcr_n" ref="mcr_n"></mcr_table>
          </div>
        </div>
      </div>
      <div v-else-if="material.type === 'Sheet'" class="group">
        <h6>Sheet</h6>
        <div class="form-group">
          <label for="thickness">Thickness</label>
          <input v-model="material.sheet.thickness" type="text" class="form-control" id="thickness">
        </div>
      </div>
      <button @click.prevent="cancel" class="btn btn-danger">Cancel</button>
      <button @click.prevent="send" class="btn btn-primary">
        {{ this.material && this.material.id ? 'Update' : "Create" }}
      </button>
    </form>
  </div>
</template>

<script>
import Mcr_table from "@/components/material/mcr_table";
export default {
  name: "material-form",
  components: {Mcr_table},
  props: {
    material: {
      type: Object,
      default: () => {
        return {profile: {}, sheet: {}, family: {}}
      }
    }
  },
  data() {
    return {
      families: [],
      types: ["Profile", "Sheet"],
      //mcr_p: {entry: []},
      //mcr_n: {entry: []},
    }
  },
  created() {
    this.$axios.$get(`/api/families/`).then(r => {
      this.families = r.map(o => {
        return {value: o.id, text: o.name}
      })
    })
  },
  computed: {
    mcr_p() {
      return this.material && this.material.profile && this.material.profile.mcr_p ? this.material.profile.mcr_p :  {entry: []}
    }
  },
  methods: {
    cancel() {
      this.$router.go(-1)
    },
    send() {
      if (this.material && this.material.id) { // material exists => needs updating
        if(this.material.type === "Profile") {
          this.material.profile.mcr_p = JSON.parse(JSON.stringify(this.$refs.mcr_p.mcr))
          this.material.profile.mcr_n = JSON.parse(JSON.stringify(this.$refs.mcr_n.mcr))

          delete this.material.sheet;
        } else if(this.material.type === "Sheet") {

          delete this.material.profile;
        }

        this.material.manufacturerUsername = "manu"
        delete this.material.type;
        this.$axios.$put(`/api/materials/${this.material.id}`, this.material).then(r => {
          console.log(r);
          this.$router.push(`/materials/${r.id}`)
        }).catch(r => {
          this.$toast.error("An error occurred. Try Again").goAway(2000)
        })




         // TODO UPDATE
      } else { // create a new material
        if(this.material.type === "Profile") {
          this.material.profile.mcr_p = JSON.parse(JSON.stringify(this.$refs.mcr_p.mcr))
          this.material.profile.mcr_n = JSON.parse(JSON.stringify(this.$refs.mcr_n.mcr))

          delete this.material.sheet;
        } else if(this.material.type === "Sheet") {

          delete this.material.profile;
        }

        this.material.manufacturerUsername = "manu"
        delete this.material.type;
        this.$axios.$post(`/api/materials/`, this.material).then(r => {
          console.log(r);
          this.$router.push(`/materials/${r.id}`)
        }).catch(r => {
          this.$toast.error("An error occurred. Try Again").goAway(2000)
        })
      }
    }
  }
}
</script>

<style scoped>
.group {
  border: 1px solid #ced4da;
  border-radius: 0.25rem;
  padding: 0.375rem 0.75rem;
}
</style>
