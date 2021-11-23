<template>
  <div v-if="posts.length > 0 ">
    <h3>List Users</h3>
    <post-item
        v-for="post in posts"
        :post="post"
        :key="post.id"
        @remove="$emit('remove',post)"
    ></post-item>
  </div>
  <h2 v-else style="color: red">
    Why did you delete all posts ?
  </h2>

</template>

<script>
import PostItem from "./PostItem";
export default {
  components: {
    PostItem
  },
  props: {
    posts: {
      type: Array,
      required: true
    }
  },

}
</script>

<style scoped>

</style>