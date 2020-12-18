<template>
  <div class="col">
    <label>MCR {{ typeLong }}</label>
    <div class="group group-table">
      <div class="input-group mb-3" v-for="(i_mcr, index) in mcr.entry">
        <input class="form-control" type="text" v-bind:value="i_mcr.key + ' → ' + i_mcr.value" readonly>
        <div class="input-group-append">
          <button class="btn btn-danger" @click.prevent="remove(index)">
            <fa :icon="['fas', 'trash']"/>
          </button>
        </div>
      </div>
    </div>
    <form @submit.prevent="add()">
      <div class="row">
        <div class="col">
          <label for="n_mcr">nº MCR {{ typeShort }}</label>
          <input type="text" ref="addmcr_n" class="form-control" id="n_mcr" v-model="add_mcr.n" placeholder="">
        </div>
        <div class="col">
          <label for="v_mcr">Value MCR {{ typeShort }}</label>
          <input type="text" class="form-control" id="v_mcr" v-model="add_mcr.v" placeholder="">
        </div>
        <div class="col col-md-2">
          <label>&nbsp;</label>
          <button class="btn btn-success form-control">
            <fa :icon="['fas', 'plus']"/>
          </button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: "mcr_table",
  props: {
    typeShort: String,
    mcr: {
      type: Object,
      default: () => {
        return {entry: []}
      }
    }
  },
 // props: ['typeShort', 'value'],
  data: function () {
    return {
      add_mcr: {
        n: '',
        v: ''
      },
      typeLong: this.typeShort === 'P' ? "Positive" : "Negative"
    };
  },
  methods: {
    add() {
      if (this.add_mcr.n && this.add_mcr.v) {
        this.mcr.entry.push({key: this.add_mcr.n, value: this.add_mcr.v})
        this.add_mcr.n = this.add_mcr.v = ''
        this.$refs.addmcr_n.focus()
      }
    },
    remove(index) {
      this.mcr.entry.splice(index, 1)
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

.group-table {
  max-height: 150px;
  overflow: auto;
}

.mb-3 {
  margin-bottom: 0.2rem !important;
}
</style>
