<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <br />
    <input type="text" v-model="search" v-bind:onchange="(e)=>fetchArtigos(e.target.value)" />
    <code> artigos: {{articles}}</code>
    <div>
      <input @change="(e)=> handleCheck(e.target.checked)" type="checkbox" id="maisRelevantes" name="maisRelevantes">
      <label for="maisRelevantes">Mais Relevantes</label>
    </div>
    <!-- a afirmação de que nenhum post foi retornado é verdadeira, então é mostrado a mensagem -->
    <div v-if="nenhumPost == true">
      <h1>Não existem artigos relacionados ao termo pesquisado!</h1>
    </div>
    <!-- Caso exista algum resultado ele sera mostrado -->
    <div v-else>
      <div v-for="post in filteredPosts" v-bind:key="post.id">
        <PostView :post="post"></PostView>
      </div>
    </div>
    <div class='pages' v-for="n in pages" v-bind:key="n">
      <a href='#' @click="pageChange(n)">{{n}}</a>
    </div>
  </div>
</template>

<script>
import PostView from "./components/PostView.vue";

export default {
  name: "App",
  components: {
    PostView,
  },
  data() {
    return {
      search: "calabazas",
      posts: [],
      pages: 0,
      articles: 0,
      maisRelevantes: false,
      nenhumPost: '',
    };
  },
  computed: {
    filteredPosts() {
      return this.posts;
    },
  },
  methods: {
    //mais relevante checado ou não no checkbox
    handleCheck(e) {
      this.maisRelevantes = (e);
      this.pageChange(this.page)
    },
    //selecionar por mais relevantes
    pageChange(page) {
      this.maisRelevantes ? (
        fetch(
          `https://api.beta.mejorconsalud.com/wp-json/mc/v2/posts?search=${this.search}&page=${page}&orderby=relevance`
        )
          .then((res) => res.json())
          .then((res) => {
            res.size === 0 ? (this.nenhumPost = true) : (this.nenhumPost = false) //caso nenhum post exista, nenhum post mostra nenhum resultado
            this.posts = res.data;
            this.pages = res.pages;
            this.articles = res.size;
          })
          .catch((err) => console.log(err))
      ) : (
        fetch(
          `https://api.beta.mejorconsalud.com/wp-json/mc/v2/posts?search=${this.search}&page=${page}`
        )
          .then((res) => res.json())
          .then((res) => {
            res.size === 0 ? (this.nenhumPost = true) : (this.nenhumPost = false)  //caso nenhum post exista, nenhum post mostra nenhum resultado
            this.posts = res.data;
            this.pages = res.pages;
            this.articles = res.size;
          })
          .catch((err) => console.log(err))
      );
    },

    fetchArtigos(search) {
      this.maisRelevantes ? (
        fetch(
          `https://api.beta.mejorconsalud.com/wp-json/mc/v2/posts?search=${search}&orderby=relevance`
        )
          .then((res) => res.json())
          .then((res) => {
            res.size === 0 ? (this.nenhumPost = true) : (this.nenhumPost = false) //caso nenhum post exista, nenhum post mostra nenhum resultado
            this.posts = res.data;
            this.pages = res.pages;
            this.articles = res.size;
          })
          .catch((err) => console.log(err))
      ) : (
        fetch(
          `https://api.beta.mejorconsalud.com/wp-json/mc/v2/posts?search=${search}`
        )
          .then((res) => res.json())
          .then((res) => {
            res.size === 0 ? (this.nenhumPost = true) : (this.nenhumPost = false) //caso nenhum post exista, nenhum post mostra nenhum resultado
            this.posts = res.data;
            this.pages = res.pages;
            this.articles = res.size;
          })
          .catch((err) => console.log(err))
      );

    },
  },
  created() {
    this.fetchArtigos(this.search = 'calabazas');
  },
};
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

.pages {
  margin-top: 20px;
  padding: 3px;
  display: inline-block;
}

* {
  padding: 0;
  margin: 0;
}
</style>
