<template>
    <div>
        <div v-if="loading">
            <Loading />
        </div>
        <div class="blog" v-else>
            <Navbar />
            <div class="container mx-auto flex flex-wrap py-6">

                <!-- Posts Section -->
                <section class="w-full md:w-2/3 flex flex-col items-center px-3">

                    <article class="flex flex-col shadow my-4">
                        <!-- Article Image -->
                        <div class="bg-white flex flex-col justify-start p-6">
                            <p v-if="data.categories" class="text-sm text-blue-500 uppercase">
                            <div v-for="i in data.categories" :key="data.categories[i]">
                                <a :href="i.link">
                                    <p class="text-blue-700 text-sm font-bold uppercase pb-4">
                                        {{i.name}}
                                    </p>
                                </a>
                            </div>
                            </p>
                            <p class="text-3xl font-bold hover:text-gray-700 pb-4">{{data.title}}</p>
                            <p href="#" class="text-sm pb-3">
                                By <a href="#" class="font-semibold hover:text-gray-800">David Grzyb</a>, Published on
                                April 25th,
                                2020
                            </p>
                            <span class="pb-6">
                                <span v-html="data.content"></span>
                            </span>
                            <div class="flex items-center mt-6">
                                <nuxt-img format="webp" placeholder
                                    class="object-cover object-center w-20 h-20 rounded-full" :src="data.author.picture"
                                    alt="" />

                                <div class="mx-4">
                                    <h1 class="text-sm text-gray-700 dark:text-gray-200"><a
                                            :href="data.author.link">{{data.author.name}}</a></h1>
                                    <details>
                                        <summary class="text-sm text-gray-500 dark:text-gray-400">Saiba Mais</summary>
                                        <p class="text-2xl m-4 text-slate-500">Bibliografia</p>
                                        <div v-html="data.bibliography"></div>
                                        <p class="text-2xl m-4 text-slate-500">Sobre o autor</p>
                                        <div v-html="data.author.description"></div>
                                    </details>
                                    <br />
                                    <div v-if="data.tag" class="content mt-6 lg:mt-0 lg:mx-6 grid">
                                        <div class="ml-6 mr-6 ">
                                            <p class="text-sm text-blue-500 uppercase">Tag:
                                            <div v-for="tag in data.tags" :key="data.tags[tag]">
                                                <a :href="i.link">
                                                    <p>
                                                        {{i.name}}
                                                    </p>
                                                </a>
                                            </div>
                                            </p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </article>

                    <article class="flex flex-col shadow my-4">
                        <!-- Next Article -->
                        <div class="bg-white flex flex-row justify-center">
                            <NextPost :data="nextPost" :img="nextPostImg" />
                            <NextPost :data="prevPost" :img="prevPostImg" />
                        </div>
                    </article>
                </section>

                <!-- Sidebar Section -->
                <aside class="w-full md:w-1/3 flex flex-col items-center px-3">

                    <div class="w-full bg-white shadow flex flex-col my-4 p-6">
                        <p class="text-xl font-semibold pb-5">About Us</p>
                        <div class="left-bar lg:-mx-6">
                            <span v-for="post in data.next_posts">
                                <PostLeftBar :id="post.id" :categories="post.categories" :titulo="post.title"
                                    :texto="post.excerpt" />
                            </span>
                        </div>
                    </div>

                    <div class="w-full bg-white shadow flex flex-col my-4 p-6">
                        <p class="text-xl font-semibold pb-5">Posts Anteriores</p>
                        <div class="left-bar lg:-mx-6">
                            <span v-for="post in data.previous_posts">
                                <PostLeftBar :id="post.id" :categories="post.categories" :titulo="post.title"
                                    :texto="post.excerpt" />
                            </span>
                        </div>
                    </div>
                </aside>
            </div>
        </div>
    </div>
</template>

<script>
import NextPost from '../components/NextPost.vue';
import PostLeftBar from '~/components/PostLeftBar.vue';
import { ContentLoader } from 'vue-content-loader'
import Loading from '~/components/Loading.vue';

export default {
    name: 'Blog',
    components: {
    NextPost,
    PostLeftBar,
    ContentLoader,
    Loading
},
    data() {
        return {
            loading: true,
            post: {
                featured_media: {
                    thumbnail: '../static/placeholder.webp'
                },
            },
            data: {
                title: 'Loading',
                content: 'Loading',
                author: {
                    picture: '../static/placeholder.webp'
                },
                featured_media: {
                    thumbnail: '../static/placeholder.webp',
                    large: '../static/placeholder.webp'
                },
                next_posts: {
                    featured_media: {
                        thumbnail: '../static/placeholder.webp',
                    }
                }
            },
            newDate: '',
            nextPost: {},
            prevPost: {},
            nextPostImg: '../static/placeholder.webp',
            prevPostImg: '../static/placeholder.webp',
        }
    },
    created() {
        this.fetchPost(this.$route.query.id);
    },
    head() {
        return {
            title: this.data.title,
            meta: [
                { name: 'title', content: this.data.title },
                { name: 'description', content: this.data.content },
                { name: 'twitter: title', content: this.data.title },
                { name: 'twitter: description', content: this.data.content },
                { name: 'twitter: url', content: `/post?id=${this.$route.query.id}` },
                { name: 'twitter: image', content: this.data.featured_media.thumbnail },
                { name: 'twitter: large', content: this.data.featured_media.large },
                { name: 'og: type', content: 'website' },
                { name: 'og: url', content: `/post?id=${this.$route.query.id}` },
                { name: 'og: title', content: this.data.title },
                { name: 'og: description', content: this.data.content },
                { name: 'og: image', content: this.data.featured_media.thumbnail },
                { name: 'viewport', content: 'width=device-width, initial-scale=1' },
                {
                    hid: this.data.content,
                    name: this.data.title,
                    content: this.data.content
                }
            ],
            link: [{ rel: 'icon', type: 'image/x-icon', href: '/favicon.ico' }],
        }
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

<style>
.main-div {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 1fr;
}

.content {
    grid-column: 2;
    grid-row: 1;
    float: left;
}

.left-bar {
    grid-column: 1;
}
</style>