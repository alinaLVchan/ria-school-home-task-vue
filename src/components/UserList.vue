<template>
  <table class="table">
    <thead>
    <tr>
      <th scope="col">#</th>
      <th scope="col">Name</th>
      <th scope="col">Email</th>
      <th scope="col">Age</th>
    </tr>
    </thead>
    <tbody>
    <tr v-for="user, index in users.slice().reverse()" :key="user.email">
      <th scope="row">{{ index + 1 }}</th>
      <td>{{ user.name | letterCases }}</td>
      <td>{{ user.email }}</td>
      <td>{{ user.age }}</td>
    </tr>
    </tbody>
  </table>
</template>

<script>
export default {
name: "UserList",
props: ["users"], user: { type: Object, required: true },
filters: {
  letterCases: function (value) {
    if (!value) return ''
    value = value.toString()
    return value.charAt(0).toUpperCase() + value.slice(1).toLowerCase()
  }
}
}
</script>

<style scoped>

</style>