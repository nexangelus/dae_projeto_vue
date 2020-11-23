<template>
  <table class="table table-striped">
    <tr>
      <th>Title</th>
      <th>Description</th>
      <th>Client</th>
      <th>Designer</th>
      <th>Created Date</th>
      <th>Updated Date</th>
    </tr>
    <tr v-for="project of projects" :key="project.id">
      <td><nuxt-link :to="`/project/${project.id}`">{{project.title}}</nuxt-link></td>
      <td>{{project.description}}</td>
      <td>{{project.clientUsername}}</td>
      <td>{{project.designerUsername}}</td>
      <td>{{$moment(project.created).format("DD/MM/YYYY HH:mm")}}</td>
      <td>{{project.updated ? $moment(project.updated).format("DD/MM/YYYY HH:mm") : ''}}</td>
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
