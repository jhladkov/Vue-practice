<template>
  <div>
    <h1>Page with posts</h1>
    <my-input
        v-focus
        v-model="searchQuery"
    />
    <div class="app__btns">
      <my-select
          v-model="selectedSort"
          :options="sortOptions"
      />
    </div>
    <my-button
        @click="showDialog"
        style="margin: 15px 0"
    >
      Create post
    </my-button>
    <my-dialog v-model:show="dialogVisible">
      <post-form
          @create="createPost"
      />
    </my-dialog>
    <post-list
        v-if="!isPostsLoading"
        :posts="sortedAndSearchedPosts"
        @remove="removePost"
    /> <!-- v-bind:Название пропса-->
    <div v-else>Please wait)</div>
    <div v-intersection="loadMorePosts" class="observer"></div>
    <!--    <div class="page__wrapper">-->
    <!--      <div-->
    <!--          class="page"-->
    <!--          :key="pageNumber"-->
    <!--          :class="{ 'current-page': page === pageNumber }"-->
    <!--          v-for="pageNumber in totalPage"-->
    <!--          @click="changePage(pageNumber)"-->
    <!--      >{{pageNumber}}</div>-->
    <!--    </div>-->
  </div>
</template>

<script>
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
import axios from 'axios'

export default {
  components: {
    PostList,
    PostForm
  },

  data() {
    return {
      posts: [],
      dialogVisible: false,
      isPostsLoading: false,
      page: 1,
      limit: 10,
      totalPage: 0,
      selectedSort: '',
      searchQuery: '',
      sortOptions: [
        {value: 'title', name: 'about title'},
        {value: 'body', name: 'about body'},
      ]
    }
  },
  methods: {
    createPost(post) { // 44.07
      this.posts.push(post)
      this.dialogVisible = false
    },
    showDialog() {
      this.dialogVisible = true
    },
    // changePage(pageNumber) {
    //   this.page = pageNumber
    // },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id)
    },
    async fetchPost() {
      try {
        this.isPostsLoading = true
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit
          }
        })
        this.totalPage = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = response.data

      } catch (err) {
        alert(err)
      } finally {
        this.isPostsLoading = false
      }
    },
    async loadMorePosts() {
      try {
        this.page += 1
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit
          }
        })
        this.totalPage = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = [...this.posts,...response.data]

      } catch (err) {
        alert(err)
      }
    },
  },
  mounted() {
    this.fetchPost()
    // const options = {
    //   rootMargin: '0px',
    //   threshold: 1.0
    // }
    // const callback = (entries, observer) => {
    //   if (entries[0].isIntersecting && this.page < this.totalPage) { // знаем пересекли мы елемент или нет
    //     this.loadMorePosts()
    //   }
    // };
    // const observer = new IntersectionObserver(callback, options);
    // observer.observe(this.$refs.observer)
  },
  computed: {
    sortedPosts() {
      return [...this.posts].sort((post1, post2) => {
        return post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
      })
    },
    sortedAndSearchedPosts() {
      return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.trim().toLowerCase()))
    }
  },
  watch: {
    // page() {
    //   this.fetchPost()
    // }
  }

}
</script>
<style>
.app__btns {
  margin: 15px 0;
  cursor: pointer;
  display: flex;
  justify-content: space-between;

}
.page__wrapper{
  display: flex;
  flex-wrap: wrap;
  margin-top: 15px;
}
.observer{
  height: 30px;
  background: green;
}
.page{
  border: 1px solid black;
  padding: 10px;
  margin-bottom: 10px;
  cursor: pointer;
  margin-right: 10px;
}
.page:last-child{
  margin-right: 0;
}

.current-page{
  border-color: red;
}

.app {
  padding: 20px;
}

</style>