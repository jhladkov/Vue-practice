<template>
  <form @submit.prevent>
    <h4>Create post</h4>
    <my-input
        v-focus
        v-model="post.title"
        type="text"
        placeholder="Enter sth"
    />
    <my-input
        v-model="post.body"
        type="text"
        placeholder="Review sth"
    />
    <my-button
        class="btn"
        @click="createPost"
    >Create
    </my-button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      post: {
        title: '',
        body: '',
      }
    }
  },
  methods: {
    createPost() {
      this.post.id = Date.now()
      this.$emit('create', this.post) // this.post - аргумент, который мы будем получать после подписки
      this.post = {
        title: '',
        body: '',
      }
    },
  },
  // watch: {
  //   post:{
  //     handler(newValue) {
  //       console.log('sdfsdf')
  //       console.log(newValue)
  //     },
  //     deep: true
  //   }
  // }
}
</script>

<style scoped>


form {
  display: flex;
  flex-direction: column;
}

</style>