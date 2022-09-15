<template>
  <div id="app">
    <img alt="Vue logo" src="../assets/logo.png" />
    <br />
    <input type="text" v-model="search" v-bind:onchange="(e)=>fetchArtigos(e.target.value, 1)" />
    <code> artigos: {{articles}}</code>
    <div>
      <input @change="(e)=> handleCheck(e.target.checked)" type="checkbox" id="maisRelevantes" name="maisRelevantes">
      <label for="maisRelevantes" class="font-bold">Mais Relevantes</label>
    </div>
    <!-- a afirmação de que nenhum post foi retornado é verdadeira, então é mostrado a mensagem -->
    <div v-if="resultadoDaPesquisa == 'nao-existe'">
      <h1>Não existem artigos relacionados ao termo pesquisado!</h1>
    </div>
    <!-- mensagem inicial -->
    <div v-else-if="resultadoDaPesquisa === 'bem-vindo'">
      <h1>Bem Vindo</h1>
    </div>
    <!-- pesquisa vazia -->
    <div v-else-if="resultadoDaPesquisa === 'caixa-vazia'">
      <h1>Digite alguma coisa.</h1>
    </div>
    <!-- Caso exista algum resultado ele sera mostrado -->
    <div v-else>
      <div v-for="post in filteredPosts" v-bind:key="post.id">
        <PostView :post="post"></PostView>
      </div>
    </div>
    <div class='pages' v-for="pageNumber in totalPages" v-bind:key="pageNumber">
      <a href='#' @click="fetchArtigos(search, pageNumber)">{{pageNumber}}</a>
    </div>
  </div>
</template>

<script>
import PostView from "../components/PostView.vue";

export default {
  name: "App",
  components: {
    PostView,
  },
  data() {
    return {
      search: "",
      posts: [],
      totalPages: 0,
      articles: 0,
      maisRelevantes: false,
      resultadoDaPesquisa: true,
      currentPage: 1,
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
      this.fetchArtigos(this.search, this.currentPage)
    },
    //selecionar por mais relevantes
    fetchArtigos(search, page) {
      if (search === '') {
        //caso o usuário não coloque nada ou apague, retorna mensagem
        (
          this.resultadoDaPesquisa = 'caixa-vazia',
          this.articles = 0,
          this.totalPages = 0
        )
      } else {
        this.resultadoDaPesquisa = false,
          this.maisRelevantes ? (
            fetch(
              `https://api.beta.mejorconsalud.com/wp-json/mc/v2/posts?search=${search}&page=${page}&orderby=relevance`
            )
              .then((res) => res.json())
              .then((res) => {
                res.size === 0 ? (this.resultadoDaPesquisa = 'nao-existe') : (this.resultadoDaPesquisa = 'mostrar-resultado') //caso nenhum post exista, nenhum post mostra nenhum resultado
                this.posts = res.data;
                this.totalPages = res.pages;
                this.articles = res.size;
                this.currentPage = page;
                console.log(this.posts)
              })
              .catch((err) => console.log(err))
          ) : (
            fetch(
              `https://api.beta.mejorconsalud.com/wp-json/mc/v2/posts?search=${search}&page=${page}`
            )
              .then((res) => res.json())
              .then((res) => {
                res.size === 0 ? (this.resultadoDaPesquisa = 'nao-existe') : (this.resultadoDaPesquisa = 'mostrar-resultado')  //caso nenhum post exista, nenhum post mostra nenhum resultado
                this.posts = res.data;
                this.totalPages = res.pages;
                this.articles = res.size;
                this.currentPage = page;
                console.log(this.posts)
              })
              .catch((err) => console.log(err))
          )
      }
    },
  },
  created() {
    this.fetchArtigos(this.search = '', 1);
    this.resultadoDaPesquisa = 'bem-vindo' //msg inicial
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
