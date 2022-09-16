

<template>
  <div id="app" class="flex justify-center align-middle">
    <Navbar />
    <img alt="Vue logo" src="../assets/logo.png" />
    <br />
    <input v-model="search" type="text" @change="(e)=>fetchArtigos(e.target.value, 1)" />
    <code> artigos: {{articles}}</code>
    <div>
      <input id="maisRelevantes" type="checkbox" name="maisRelevantes" @change="(e)=> handleCheck(e.target.checked)">
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
    <div v-else-if="resultadoDaPesquisa === 'carregando'">
      <h1>Loading</h1>
    </div>
    <!-- pesquisa vazia -->
    <div v-else-if="resultadoDaPesquisa === 'caixa-vazia'">
      <h1>Digite alguma coisa.</h1>
    </div>
    <!-- Caso exista algum resultado ele sera mostrado -->
    <div v-else>
      <div v-for="post in filteredPosts" :key="post.id" class=''>
        <PostView :post="post"></PostView>
      </div>
    </div>
   <!--  <div v-for="pageNumber in totalPages" :key="pageNumber" class='pages'>
      <a href='#' @click="fetchArtigos(search, pageNumber)">{{pageNumber}}</a>
    </div> -->
    <div class="overflow-auto">
      <b-pagination-nav :link-gen="linkGen" :number-of-pages="totalPages"></b-pagination-nav>
    </div>
  </div>
</template>

<script>
import PostView from "../components/PostView.vue";
export default {
  name: 'IndexPage',
  components: {
    PostView,
  },
  data() {
    return {
      search: "",
      posts: [],
      totalPages: 1,
      articles: 0,
      maisRelevantes: false,
      resultadoDaPesquisa: true,
      currentPage: 1,
      itemsPerPage: 1,
      resultCount: 0,
      pageNum: 1
    };
  },
    watch: {

  },
  computed: {
    page() {
      this.$route.query.page
    },
    filteredPosts() {
      this.fetchArtigos(this.search, this.$route.query.page);
      return this.posts;
    },
  },
  created() {
    this.resultadoDaPesquisa = 'bem-vindo' //msg inicial
  },
  methods: {
    linkGen(page) {
      return {
        query: { page: page },
      }
    },
    //mais relevante checado ou não no checkbox
    handleCheck(e) {
      this.maisRelevantes = (e);
      this.fetchArtigos(this.search, this.currentPage)
    },
    //selecionar por mais relevantes
    fetchArtigos(search, page) {
     console.log(search + '' + page)
      if (search === '') {
        //caso o usuário não coloque nada ou apague, retorna mensagem
        (
          this.resultadoDaPesquisa = 'caixa-vazia',
          this.articles = 0,
          this.totalPages = 0
        )
      } else {
        this.resultadoDaPesquisa = 'carregando',
          this.maisRelevantes ? (
            fetch(
              `https://api.beta.mejorconsalud.com/wp-json/mc/v2/posts?search=${search}&page=${page|1}&orderby=relevance`
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
              .then(this.resultadoDaPesquisa = 'mostrar')
              .catch((err) => console.log(err))
          ) : (
            this.resultadoDaPesquisa = 'carregando',
            fetch(
              `https://api.beta.mejorconsalud.com/wp-json/mc/v2/posts?search=${search}&page=${page|1}`
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
              .then(this.resultadoDaPesquisa = 'mostrar')
              .catch((err) => console.log(err))
          )
      }
    },
  },
};
</script>

<style scoped>
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

a {
  color: #999;
}

.current {
  color: red;
}

ul {
  padding: 0;
  list-style-type: none;
}

li {
  display: inline;
  margin: 5px 5px;
}

a.first::after {
  content: '...'
}

a.last::before {
  content: '...'
}
</style>