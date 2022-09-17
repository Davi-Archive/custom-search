<template>
  <div>
    <div id="app" class="app">
      <Navbar />
      <div class="flex">
        <img alt="Search Logo" src="../assets/logo.svg" />
        <br />
        <div class="-space-x-2 mx-auto w-max relative">
          <input
            class="peer bg-stone-200 h-10 md:h-12 pl-14 text-xl font-semibold text-blue-700 focus:bg-stone-300 outline-none caret-blue-700"
            type="text" v-model="search" @onchange="(e)=>setArtigos(search, 1)" />
          <svg class="w-5 absolute top-1/2 -translate-y-1/2 left-5 fill-blue-700 peer-focus-within:fill-blue-900"
            xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
            <path
              d="M500.3 443.7l-119.7-119.7c27.22-40.41 40.65-90.9 33.46-144.7C401.8 87.79 326.8 13.32 235.2 1.723C99.01-15.51-15.51 99.01 1.724 235.2c11.6 91.64 86.08 166.7 177.6 178.9c53.8 7.189 104.3-6.236 144.7-33.46l119.7 119.7c15.62 15.62 40.95 15.62 56.57 0C515.9 484.7 515.9 459.3 500.3 443.7zM79.1 208c0-70.58 57.42-128 128-128s128 57.42 128 128c0 70.58-57.42 128-128 128S79.1 278.6 79.1 208z" />
          </svg>
          <button class="bg-blue-800 hover:bg-blue-900 text-white font-semibold text-lg h-10 md:h-12 px-10 md:px-12" @click="(e)=>setArtigos(search, 1)">
            Pesquisar
          </button>
        </div>
        <div>
          <input id="maisRelevantes" type="checkbox" name="maisRelevantes" @change="(e)=> handleCheck(e.target.checked)">
          <label for="maisRelevantes" class="font-bold">Mais Relevantes</label>
        </div>
        <code> artigos: {{articles}}</code>
      </div>
      <!-- a afirmação de que nenhum post foi retornado é verdadeira, então é mostrado a mensagem -->
      <div v-if="resultadoDaPesquisa == 'nao-existe'">
        <h1>Não existem artigos relacionados ao termo pesquisado!</h1>
      </div>
      <!-- mensagem inicial -->
      <div v-else-if="resultadoDaPesquisa === 'bem-vindo'">
        <h1>Bem Vindo</h1>
      </div>
      <div v-else-if="$fetchState.pending" class="mt-40 mb-40">
        <button disabled type="button"
          class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-md text-sm px-5 py-2.5 text-center mr-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800 inline-flex items-center">
          <svg role="status" class="inline mr-3 w-8 h-8 text-white animate-spin rounded-r-md" viewBox="0 0 100 101" fill="none"
            xmlns="http://www.w3.org/2000/svg">
            <path
              d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z"
              fill="#E5E7EB" />
            <path
              d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z"
              fill="currentColor" />
          </svg>
          <span class="text-4xl font-serif">Carregando</span>
        </button>
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
    </div>



    <div class="flex items-center justify-between border-t border-gray-200 bg-white px-4 py-3 sm:px-6">
      <div class="flex flex-1 justify-between sm:hidden">
        <a href="#"
          class="relative inline-flex items-center rounded-md border border-gray-300 bg-white px-4 py-2 text-sm font-medium text-gray-700 hover:bg-gray-50">Previous</a>
        <a href="#"
          class="relative ml-3 inline-flex items-center rounded-md border border-gray-300 bg-white px-4 py-2 text-sm font-medium text-gray-700 hover:bg-gray-50">Next</a>
      </div>
      <div class="hidden sm:flex sm:flex-1 sm:items-center sm:justify-between">
        <div v-if="totalPages>0">
          <p class="text-sm text-gray-700">
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
        <div v-else class="text-sm text-gray-700 font-medium">
          Nenhuma página para exibir
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
                v-model="pageInputSearch" />
              <button @click="writePage(pageInputSearch)">
                <outline-search-icon
                  class="w-10 h-10 relative inline-flex items-center rounded-l-md border border-gray-300 bg-white text-gray-500 hover:bg-gray-50 focus:z-20" />
              </button>
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

Vue.use(VueTailwind)


export default {
  name: 'IndexPage',
  components: {
    PostView,
  },
  async fetch() {
    await this.Artigos(this.search, this.currentPage)
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
      pageInputSearch: this.currentPage,
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
        this.$fetch()
        this.Artigos(this.search, this.totalPages)
      }
      else if (1 > newPage) {
        Swal.fire({
          title: 'Erro!',
          text: `A pesquisa tem que começar da página 1.`,
          icon: 'error',
          confirmButtonText: 'Retornar'
        })
        this.Artigos(this.search, 1)
        return
      }
      else if (newPage = this.currentPage) {
        this.$fetch()
        this.Artigos(this.search, this.currentPage)
      }
      else {
        this.$fetch()
        this.Artigos(this.search, newPage)
      }
    }
  },
  created() {
    this.resultadoDaPesquisa = 'bem-vindo' //msg inicial
  },
  methods: {
    setArtigos(search, page) {
      this.search = search;
      this.currentPage = page;
      this.$fetch()
    },
    writePage(page) {   //on change text
      this.setArtigos(this.search, page) // setartigo para fetch hook usar o Artigos()
    },
    backPage(page) {   //botão de voltar - paginação
      page--
      this.setArtigos(this.search, page) // setartigo para fetch hook usar o Artigos()
    },
    forwardPage(page) {  //botão de avançar - paginação
      page++
      this.setArtigos(this.search, page)
    },
    //mais relevante checado ou não no checkbox
    handleCheck(e) {
      this.maisRelevantes = (e);
      this.setArtigos(this.search, this.currentPage)
    },
    //selecionar por mais relevantes
    async Artigos(search, page) {
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
            this.$axios.$get(
              `https://api.beta.mejorconsalud.com/wp-json/mc/v2/posts?search=${search}&page=${page}&orderby=relevance`
            ).then((res) => {
              res.size === 0 ? (this.resultadoDaPesquisa = 'nao-existe') : (this.resultadoDaPesquisa = 'mostrar-resultado') //caso nenhum post exista, nenhum post mostra nenhum resultado
              this.posts = res.data;
              this.totalPages = res.pages;
              this.articles = res.size;
              this.currentPage = page;
              this.pageInputSearch = page;
              console.log(this.posts)
            })
              .then(this.resultadoDaPesquisa = 'mostrar', this.$nuxt.$loading.finish())
              .catch((err) => console.log(err))
          ) : (
            this.resultadoDaPesquisa = 'carregando',
            await this.$axios.$get(
              `https://api.beta.mejorconsalud.com/wp-json/mc/v2/posts?search=${search}&page=${page}`
            ).then((res) => {
              res.size === 0 ? (this.resultadoDaPesquisa = 'nao-existe') : (this.resultadoDaPesquisa = 'mostrar-resultado')  //caso nenhum post exista, nenhum post mostra nenhum resultado
              this.posts = res.data;
              this.totalPages = res.pages;
              this.articles = res.size;
              this.currentPage = page;
              this.pageInputSearch = page;
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
  width: 55px;
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

.animate-spin {
  animation: spin 1s linear infinite;
}

@keyframes spin {
  from {
    transform: rotate(0deg);
  }

  to {
    transform: rotate(360deg);
  }
}
</style>