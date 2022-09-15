<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <input type="text" v-model="search" />
    <div v-for="post in filteredPosts" v-bind:key="post.id">
      <PostView :post="post"></PostView>
    </div>
  </div>
</template>

<script>
import PostView from './components/PostView.vue';


export default {
  name: 'App',
  components: {
    PostView,
  },
  data() {
    return {
      search: '',
      posts: [
        {
          id: 1,
          title: 'Post 1',
          body: 'Vue.js is cool'
        },
        {
          id: 2,
          title: 'Post 2',
          body: 'Javascript is cool'
        },
        {
          id: 3,
          title: 'Post 3',
          body: 'React is cool'
        },
      ]
    }
  },
  computed: {
    filteredPosts(){
      return this.posts.filter(post => post.body.toLowerCase().includes(this.search.toLowerCase()))
    }
  }
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
