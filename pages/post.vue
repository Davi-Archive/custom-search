<template>
    <div>
        <Navbar />
        <div v-if="loading">
            Loading..
        </div>
        <div class="blog" v-else>
            <h1>{{data.title}}</h1>
            <h3>{{data.headline}}</h3>
            <div>
                Categorias:
                <div v-for="i in data.categories" :key="data.categories[i]">
                    <a :href="i.link">
                        <p>
                            {{i.name}}
                        </p>
                    </a>
                </div>
                Tags:
                <div v-for="tag in data.tags" :key="data.tags[tag]">
                    <a :href="i.link">
                        <p>
                            {{i.name}}
                        </p>
                    </a>
                </div>
            </div>
            <details>
                <summary>Escrito por: <a :href="data.author.link">{{data.author.name}}</a></summary>
                <img :src="data.author.picture" />
                <div v-html="data.author.description"></div>
            </details>
            <div>
                Ultima atualização: {{newDate}}
            </div>
        </div>



        <!-- component -->
        <div class="bg-white dark:bg-gray-900">
            <div class="container px-6 py-10 mx-auto">
                <h1 class="text-3xl font-semibold text-gray-800 capitalize lg:text-4xl dark:text-white">{{data.title}}
                </h1>
                <div class="mt-8 lg:-mx-6 lg:flex lg:items-center">
                    <!-- <img class="object-cover w-full lg:mx-6 lg:w-1/2 rounded-xl h-72 lg:h-96"
                        src="https://images.unsplash.com/photo-1590283603385-17ffb3a7f29f?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1470&q=80"
                        alt="" /> -->
                        

<NextPost :data="nextPost" :img="nextPostImg" />
<NextPost :data="prevPost" :img="prevPostImg" />


                    <div class="mt-6 lg:mt-0 lg:mx-6 grid">
                        <div class="ml-6 mr-6 flex">
                            <p class="text-sm text-blue-500 uppercase w-1/2">Tag:
                            <div v-for="tag in data.tags" :key="data.tags[tag]">
                                <a :href="i.link">
                                    <p>
                                        {{i.name}}
                                    </p>
                                </a>
                            </div>
                            </p>
                            <p class="text-sm text-blue-500 uppercase w-1/2">categorias:
                            <div v-for="i in data.categories" :key="data.categories[i]">
                                <a :href="i.link">
                                    <p>
                                        {{i.name}}
                                    </p>
                                </a>
                            </div>
                            </p>

                        </div>

                        <p class="mt-3 text-sm text-gray-500 dark:text-gray-300 md:text-sm" v-html="data.content">
                        </p>
                        <div class="flex items-center mt-6">
                            <img class="object-cover object-center w-20 h-20 rounded-full" :src="data.author.picture"
                                alt="">

                            <div class="mx-4">
                                <h1 class="text-sm text-gray-700 dark:text-gray-200"><a
                                        :href="data.author.link">{{data.author.name}}</a></h1>
                                <details>
                                    <summary class="text-sm text-gray-500 dark:text-gray-400">Saiba Mais</summary>
                                    <div v-html="data.author.description"></div>
                                </details>

                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <NextPost :data="nextPost" :img="nextPostImg" />
        <NextPost :data="prevPost" :img="prevPostImg" />
    </div>
</template>

<script>
import NextPost from '../components/NextPost.vue';

export default {
    name: 'Blog',
    components: {
        NextPost,
    },
    data() {
        return {
            loading: true,
            data: {
                author: {
                    picture: '../assets/logo.png'
                }
            },
            newDate: '',
            nextPost: {},
            prevPost: {},
            nextPostImg: '../assets/logo.png',
            prevPostImg: '../assets/logo.png'

        }
    },
    created() {
        this.fetchPost(this.$route.query.id);
    },
    methods: {
        fetchPost(id) {
            this.$axios.$get(`https://api.beta.mejorconsalud.com/wp-json/mc/v1/posts/${id}`)
                .then((res) => {
                    this.data = res;
                    this.nextPost = res.next_post;
                    this.nextPostImg = res.next_post.featured_media.medium
                    this.prevPostImg = res.previous_post.featured_media.medium
                    this.prevPost = res.previous_post;
                    ;
                    this.loading = false
                    console.log(this.data);
                    //converte data para formato brasileiro
                    if (!res.reviewed) {
                        this.newDate = 'Não informado'
                    } else {
                        this.newDate = new Date(res.reviewed).toLocaleDateString("pt-BR", {
                            year: "numeric",
                            month: "2-digit",
                            day: "2-digit",
                        })
                    }
                })
                .catch((err) => console.log(err))
        }
    },
}
</script>