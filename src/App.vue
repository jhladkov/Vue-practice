<template>
  <div class="app">
    <post-form
        @create="createPost"
    />
    <post-list
        :posts="posts"
        @remove="removePost"
    /> <!-- v-bind:Название пропса-->
  </div>
</template>

<script>
import PostForm from "./components/PostForm";
import PostList from "./components/PostList";

export default {
  components: {
    PostList,
    PostForm
  },

  data() {
    return {
      posts: [
        {id: 1, title: 'JavaScript', body: 'Review about JS'},
        {id: 2, title: 'JavaScript 2', body: 'Review about JS'},
        {id: 3, title: 'JavaScript 3', body: 'Review about JS'},
        {id: 4, title: 'JavaScript 4', body: 'Review about JS'},
      ],
    }
  },
  methods: {
    createPost(post) { // 44.07
      this.posts.push(post)
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id)
    },
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app {
  padding: 20px;
}

</style>