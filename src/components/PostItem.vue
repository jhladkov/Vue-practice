<template>
  <div class="post">
    <div>
      <div><strong>Name:</strong> {{ post.title }}</div>
      <div><strong>Review:</strong> {{ post.body }}</div>
    </div>

    <div class="post__btns">
      <my-button
          style="margin-top: 0;"
          @click="$emit('remove', post)"
      >Delete</my-button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    post: {
      type:Object,
      required: true,
    }
  }
}
</script>

<style scoped>
.post {
  padding: 15px;
  border: 3px solid teal;
  margin-top: 15px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>