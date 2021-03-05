<template>
  <div class="alert alert-danger" role="alert" v-if="errors.length">
    <div li v-for="error in errors" :key="error.field">
      {{ error.field }}: {{ error.message }}
    </div>
  </div>
</template>

<script>
export default {
  name: "Error",
  props: ['errors'], error: { type: Object, required: true }
}
</script>

<style scoped>

</style>