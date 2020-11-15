<template>
  <table class="table table-striped">
    <tr>
      <th>Título</th>
      <th>Descrição</th>
      <th>Cliente</th>
      <th>Projetista</th>
      <th>Data Criação</th>
      <th>Data Atualização</th>
    </tr>
    <tr v-for="project of projects" :key="project.id">
      <td>{{project.title}}</td>
      <td>{{project.description}}</td>
      <td>{{project.client.name}}</td>
      <td>{{project.designer.name}}</td>
      <td>{{project.created}}</td>
      <td>{{project.updated}}</td>
    </tr>
  </table>
</template>

<script>
export default {
  props: ['projects']
}
</script>

<style scoped>

</style>
