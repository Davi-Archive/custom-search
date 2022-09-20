<template>
  <div>
    <Navbar />
    <div class="ml-8">
      <div class="mt-4">
        <div class="flex flex-wrap">
          <div class="flex items-center p-6 bg-white rounded-md shadow-sm w-full sm:w-1/2 xl:w-1/3 -mr-60 ml-40">
            <nuxt-img
            format="webp"
            alt="Search Logo"
            src="logo.svg"
            />
          </div>
          <div class="w-full px-6 mt-6 sm:w-1/2 xl:w-1/3 sm:mt-0">
            <div class="flex items-center px-5 py-6 bg-white rounded-md shadow-sm">
              <div class="-space-x-2 mx-auto w-max relative flex flex-row">
                <input
                  class="peer bg-stone-200 h-10 md:h-12 pl-14 text-xl font-semibold text-blue-700 focus:bg-stone-300 outline-none caret-blue-700"
                  type="text" v-model="search" @onchange="(e)=>setArtigos(search, 1)" />
                <svg class="w-5 absolute top-1/2 -translate-y-1/2 left-5 fill-blue-700 peer-focus-within:fill-blue-900"
                  xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
                  <path
                    d="M500.3 443.7l-119.7-119.7c27.22-40.41 40.65-90.9 33.46-144.7C401.8 87.79 326.8 13.32 235.2 1.723C99.01-15.51-15.51 99.01 1.724 235.2c11.6 91.64 86.08 166.7 177.6 178.9c53.8 7.189 104.3-6.236 144.7-33.46l119.7 119.7c15.62 15.62 40.95 15.62 56.57 0C515.9 484.7 515.9 459.3 500.3 443.7zM79.1 208c0-70.58 57.42-128 128-128s128 57.42 128 128c0 70.58-57.42 128-128 128S79.1 278.6 79.1 208z" />
                </svg>
                <button
                  class="bg-blue-800 hover:bg-blue-900 text-white font-semibold text-lg h-10 md:h-12 px-10 md:px-12"
                  @click="(e)=>setArtigos(search, 1)">
                  Pesquisar
                </button>
              </div>
            </div>
          </div>

          <div class="w-full px-6 mt-6 sm:w-1/2 xl:w-1/3 xl:mt-0">
            <div class="flex items-center px-5 py-6 bg-white rounded-md shadow-sm">
              <div class="ml-20 bg-green-500 bg-opacity-75 rounded-full uppercase">
                <div
                  class='flex flex-none items-center justify-center w-10 h-10 rounded-full bg-green-500'>
                  123
                </div>
              </div>
              <div class="p-3 bg-green-500 bg-opacity-75 rounded-full uppercase">
                <h4 class="text-2xl font-semibold text-gray-700">artigos: <span
                    class="bg-orange-300 rounded-full pl-3 pr-3 pb-1">{{articles}}</span></h4>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="mt-8"></div>

      <div class="flex flex-col mt-8">
        <div class="py-2 -my-2 overflow-x-auto sm:-mx-6 sm:px-6 lg:-mx-8 lg:px-8">
          <div
            class="inline-block min-w-full overflow-hidden align-middle border-b border-gray-200 shadow sm:rounded-lg">
            <client-only>
            <table class="min-w-full">
              <thead>
                <tr>
                  <th
                    class="px-6 py-3 text-xs font-medium leading-4 tracking-wider text-left text-gray-500 uppercase border-b border-gray-200 bg-gray-50 flex align-middle justify-center">
                    <div>
                      <input id="maisRelevantes" type="checkbox" name="maisRelevantes"
                        @change="(e)=> handleCheck(e.target.checked)">
                      <label for="maisRelevantes" class="font-bold">Mais Relevantes</label>
                    </div>
                  </th>
                </tr>
              </thead>
              <!-- HERE BUSCA -->
              <tbody class="bg-white">
                <tr v-if="resultadoDaPesquisa == 'nao-existe'" class="text-center text-6xl font-serif text-red-300">
                  <h1>Não existem artigos relacionados ao termo pesquisado!</h1>
                </tr>
                <!-- mensagem inicial -->
                <tr v-else-if="resultadoDaPesquisa === 'bem-vindo'"
                  class="text-center text-6xl font-serif text-green-400">
                  <h1>Bem Vindo</h1>
                </tr>
                <tr v-else-if="$fetchState.pending" class="mt-40 mb-40 p-60 text-center">
                  <button disabled type="button"
                    class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-md text-sm px-5 py-2.5 text-center mr-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800 inline-flex items-center">
                    <svg role="status" class="inline mr-3 w-8 h-8 text-white animate-spin rounded-r-md"
                      viewBox="0 0 100 101" fill="none" xmlns="http://www.w3.org/2000/svg">
                      <path
                        d="M100 50.5908C100 78.2051 77.6142 100.591 50 100.591C22.3858 100.591 0 78.2051 0 50.5908C0 22.9766 22.3858 0.59082 50 0.59082C77.6142 0.59082 100 22.9766 100 50.5908ZM9.08144 50.5908C9.08144 73.1895 27.4013 91.5094 50 91.5094C72.5987 91.5094 90.9186 73.1895 90.9186 50.5908C90.9186 27.9921 72.5987 9.67226 50 9.67226C27.4013 9.67226 9.08144 27.9921 9.08144 50.5908Z"
                        fill="#E5E7EB" />
                      <path
                        d="M93.9676 39.0409C96.393 38.4038 97.8624 35.9116 97.0079 33.5539C95.2932 28.8227 92.871 24.3692 89.8167 20.348C85.8452 15.1192 80.8826 10.7238 75.2124 7.41289C69.5422 4.10194 63.2754 1.94025 56.7698 1.05124C51.7666 0.367541 46.6976 0.446843 41.7345 1.27873C39.2613 1.69328 37.813 4.19778 38.4501 6.62326C39.0873 9.04874 41.5694 10.4717 44.0505 10.1071C47.8511 9.54855 51.7191 9.52689 55.5402 10.0491C60.8642 10.7766 65.9928 12.5457 70.6331 15.2552C75.2735 17.9648 79.3347 21.5619 82.5849 25.841C84.9175 28.9121 86.7997 32.2913 88.1811 35.8758C89.083 38.2158 91.5421 39.6781 93.9676 39.0409Z"
                        fill="currentColor" />
                    </svg>
                    <span class="text-4xl font-serif">Carregando</span>
                  </button>
                </tr>
                <!-- pesquisa vazia -->
                <tr v-else-if="resultadoDaPesquisa === 'caixa-vazia'" class="text-center text-6xl font-serif">
                  <p>Digite alguma coisa.</p>
                </tr>
                <!-- Caso exista algum resultado ele sera mostrado -->
                <tr v-else v-for="(post, index) in filteredPosts" :key="index">
                  <td class="py-4 border-b border-gray-200">
                    <div class="flex items-center">
                      <div class="flex-shrink-0 w-10 h-10">
                        <nuxt-img
                        format="webp"
                        placeholder="logo.png"
                        class="w-10 h-10 rounded-full"
                        :src="buildImageUrl(post.featured_media.medium)" alt="" />
                      </div>

                      <div class="ml-4">
                        <div class="text-sm font-medium leading-5 text-gray-900 whitespace-nowrap">
                          <PostView :post="post"></PostView>
                        </div>
                      </div>
                    </div>
                  </td>
                </tr>
                <tr span="6">
                  <div
                    class="flex items-center justify-between border-t bg-gray-400 rounded-md border-gray-200 px-4 py-3 sm:px-6 ml-10">
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
                      <div class="text-right">
                        <div class="isolate inline-flex -space-x-px rounded-md shadow-sm" aria-label="Pagination">
                          <a href="#"
                            class="relative inline-flex items-center rounded-l-md border border-gray-300 bg-white px-2 py-2 text-sm font-medium text-gray-500 hover:bg-gray-50 focus:z-20"
                            @click="backPage(currentPage)">
                            <div class="w-5 h-5">
                              &lt;-
                            </div>
                          </a>
                          <div>
                            <input id='procurarInput' type="text" width="5px"
                              class="relative z-10 inline-flex items-center border border-indigo-500 bg-indigo-50 px-4 py-2 text-sm font-medium text-indigo-600 focus:z-20"
                              v-model="pageInputSearch" />
                            <button @click="writePage(pageInputSearch)">
                              <div
                                class="w-10 h-10 relative inline-flex items-center rounded-l-md border border-gray-300 bg-white text-gray-500 hover:bg-gray-50 focus:z-20" >
                              SS
                              </div>
                            </button>
                          </div>
                          <a href="#"
                            class="relative inline-flex items-center rounded-r-md border border-gray-300 bg-white px-2 py-2 text-sm font-medium text-gray-500 hover:bg-gray-50 focus:z-20"
                            @click="forwardPage(currentPage)">
                            <span class="sr-only">Next</span>
                            <div class="w-5 h-5" >
                              ->
                            </div>
                          </a>
                        </div>
                      </div>
                    </div>
                  </div>
                </tr>
              </tbody>
            </table>
          </client-only>
          </div>
        </div>
      </div>
    </div>
  </div>

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
  head() {
    return {
      title: `Mejor com Salud`,
      meta: [
        { name: 'title', content: 'Home - Mejor com Salud' },
        { name: 'description', content: 'Pesquisar no site Mejor con Salud' },
        { name: 'twitter: title', content: `Mejor com Salud` },
        { name: 'twitter: description', content: 'Buscar no banco de dados Mejor com Salud' },
        { name: 'twitter: url', content: `/` },
        { name: 'twitter: image', content: '../static/logo.png' },
        { name: 'twitter: large', content: ''},
        { name: 'og: type', content: 'website' },
        { name: 'og: url', content: `/` },
        { name: 'og: description', content: 'Pesquisa' },
        { name: 'og: image', content: '../static/logo.png' },
        {
          hid: this.posts.content,
          name: 'Home - Mejor com Salud',
          content: this.posts[0].content
        }
      ],
      link: [{ rel: 'icon', type: 'image/x-icon', href: '/favicon.ico' }],
    }
  },
  async fetch() {
    await this.Artigos(this.search, this.currentPage)
  },
  data() {
    return {
      search: "",
      posts: [{
        featured_media:{
          large: '../static/logo.png',
          medium: '../static/logo.png',
          thumbnail: '../static/logo.png',
        },
        content: 'Mejor com salud busca'
      }],
      totalPages: 1,
      articles: 0,
      maisRelevantes: false,
      resultadoDaPesquisa: true,
      currentPage: 1,
      pageInputSearch: this.currentPage,
      itemsPerPage: 1,
      resultCount: 0,
      pageNum: '',
      img: '../static/logo.png'
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
    //build async image
    buildImageUrl(image) {
      if (!image) return "https://localhost:9045/static/logo.png";
      return `${image}`
    },
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
              .then(this.resultadoDaPesquisa = 'mostrar')
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
              .then(this.resultadoDaPesquisa = 'mostrar')
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
  max-width: 100%;
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