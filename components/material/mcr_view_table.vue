<template>
  <div class="scroll">
    <tr v-if="val.entry" v-for="entry in val.entry">
      <td>{{ `${entry.key} →  ${entry.value}` }}</td>
    </tr>
  </div>
</template>

<script>
export default {
  props: ['val'],
  name: "mcr_view_table"
}
</script>

<style scoped>
.scroll {
  /*max-height: 200px;
  overflow: auto;*/
}
</style>
