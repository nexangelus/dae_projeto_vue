<template>
  <div>
    <table class="table table-striped"> TODO TABLE
      <tr>
        <th>Title</th>
        <th>Description</th>
        <th>Client</th>
        <th>Designer</th>
        <th>Created Date</th>
        <th>Updated Date</th>
        <th>Actions</th>
      </tr>
      <tr v-for="project of projects" :key="project.id">
        <td>{{project.title}}</td>
        <td>{{project.description}}</td>
        <td>{{project.clientUsername}}</td>
        <td>{{project.designerUsername}}</td>
        <td>{{$moment(project.created).format('L LT')}}</td>
        <td>{{project.updated ? $moment(project.updated).format("L LT") : ''}}</td>
        <td>
          <nuxt-link class="btn btn-primary" :to="`/project/${project.id}`"><fa :icon="['fas', 'info']" /></nuxt-link>
        </td>
      </tr>
    </table>
  </div>
</template>
<script>
export default {
  props: ['projects']
}
</script>

<style scoped>

</style>
