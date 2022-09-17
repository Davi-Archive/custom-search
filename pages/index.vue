<template>
  <div>
    <div id="app" class="app">
      <Navbar />
      <img alt="Vue logo" src="../assets/logo.png" />
      <br />
      <input v-model="search" type="text" @change="(e)=>fetch(search, 1)" />
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
      <!-- <div v-for="pageNumber in totalPages" :key="pageNumber" class='pages'>
      <a href='#' @click="fetch(search, pageNumber)">{{pageNumber}}</a>
    </div> -->
    </div>



    <div class="flex items-center justify-between border-t border-gray-200 bg-white px-4 py-3 sm:px-6">
      <div class="flex flex-1 justify-between sm:hidden">
        <a href="#"
          class="relative inline-flex items-center rounded-md border border-gray-300 bg-white px-4 py-2 text-sm font-medium text-gray-700 hover:bg-gray-50">Previous</a>
        <a href="#"
          class="relative ml-3 inline-flex items-center rounded-md border border-gray-300 bg-white px-4 py-2 text-sm font-medium text-gray-700 hover:bg-gray-50">Next</a>
      </div>
      <div class="hidden sm:flex sm:flex-1 sm:items-center sm:justify-between">
        <div>
          <p class="text-sm text-gray-700">
            Mostrando página
            {{ ' ' }}
            <span class="font-medium">{{currentPage}}</span>
            {{ ' ' }}
            de
            {{ ' ' }}
            <span class="font-medium">{{totalPages}}</span>
            {{ ' ' }}
            Páginas
          </p>
        </div>
        <div>
          <nav class="isolate inline-flex -space-x-px rounded-md shadow-sm" aria-label="Pagination">
            <a href="#"
              class="relative inline-flex items-center rounded-l-md border border-gray-300 bg-white px-2 py-2 text-sm font-medium text-gray-500 hover:bg-gray-50 focus:z-20"
              @click="backPage(currentPage)">
              <outline-arrow-left-icon class="w-5 h-5" />
            </a>
            <div>
              <input id='procurarInput' type="text" width="5px"
                class="relative z-10 inline-flex items-center border border-indigo-500 bg-indigo-50 px-4 py-2 text-sm font-medium text-indigo-600 focus:z-20"
                v-model="currentPage" @onChange="fetch(search, currentPage)" />
            </div>
            <a href="#"
              class="relative inline-flex items-center rounded-r-md border border-gray-300 bg-white px-2 py-2 text-sm font-medium text-gray-500 hover:bg-gray-50 focus:z-20"
              @click="forwardPage(currentPage)">
              <span class="sr-only">Next</span>
              <outline-arrow-right-icon class="w-5 h-5" />
            </a>
          </nav>
        </div>
      </div>
    </div>
  </div> <!-- //root -->
</template>

<script>
import PostView from "../components/PostView.vue";
import Vue from 'vue';
import VueTailwind from 'vue-tailwind';
import Swal from 'sweetalert2';
import { async } from "q";

Vue.use(VueTailwind)


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
      pageNum: ''
    };
  },
  computed: {
    filteredPosts() {
      return this.posts;
    },
  },
  watch: {
    // whenever question changes, this function will run
    currentPage(newPage, oldPage) {
      if (newPage == '') {
        return
      }
      else if (newPage > this.totalPages) {
        Swal.fire({
          title: 'Erro!',
          text: `A pesquisa tem ${this.totalPages} páginas.`,
          icon: 'error',
          confirmButtonText: 'Retornar'
        })
        this.fetch(this.search, this.totalPages)
      }
      else if (1 > newPage) {
        Swal.fire({
          title: 'Erro!',
          text: `A pesquisa tem que começar da página 1.`,
          icon: 'error',
          confirmButtonText: 'Retornar'
        })
        this.fetch(this.search, 1)
        return
      }
      else if (newPage = this.currentPage) {
        this.fetch(this.search, this.currentPage)
      }
      else {
        this.fetch(this.search, newPage)
      }
    }
  },
  created() {
    this.resultadoDaPesquisa = 'bem-vindo' //msg inicial
  },
  methods: {
    backPage(page) {   //botão de voltar - paginação
      console.log(page--)
      this.fetch(this.search, page)
    },
    forwardPage(page) {  //botão de avançar - paginação
      page++
      this.fetch(this.search, page)
    },
    //mais relevante checado ou não no checkbox
    handleCheck(e) {
      this.maisRelevantes = (e);
      this.fetch(this.search, this.currentPage)
    },
    //selecionar por mais relevantes
    async fetch(search, page) {
      console.log(search + ' ' + page)
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
            this.$nuxt.$loading.start(),
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
              .then(this.resultadoDaPesquisa = 'mostrar', this.$nuxt.$loading.finish())
              .catch((err) => console.log(err))
          ) : (
            this.resultadoDaPesquisa = 'carregando',
            fetch(
              `https://api.beta.mejorconsalud.com/wp-json/mc/v2/posts?search=${search}&page=${page}`
            )
              .then(this.$nuxt.$loading.start())
              .then((res) => res.json())
              .then((res) => {
                res.size === 0 ? (this.resultadoDaPesquisa = 'nao-existe') : (this.resultadoDaPesquisa = 'mostrar-resultado')  //caso nenhum post exista, nenhum post mostra nenhum resultado
                this.posts = res.data;
                this.totalPages = res.pages;
                this.articles = res.size;
                this.currentPage = page;
                console.log(this.posts)
              })
              .then(this.resultadoDaPesquisa = 'mostrar', this.$nuxt.$loading.finish())
              .catch((err) => console.log(err))
          )
      }
    },
  },
};
</script>

<style>
@tailwind base;
@tailwind components;
@tailwind utilities;

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#procurarInput {
  width: 60px;
  text-align: center;
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