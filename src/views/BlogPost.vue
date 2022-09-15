<template>
    <div class="blogpost">
        <h1>{{data.headline}}</h1>
        <div>
            Categorias:
            <div v-for="i in data.categories" v-bind:key="data.categories[i]">
                <a :href="i.link">
                    <p>
                        {{i.name}}
                    </p>
                </a>
            </div>
        </div>
        <details>
            <summary>Autor: <a :href="data.author.link">{{data.author.name}}</a>, id:{{data.author.id}}</summary>
            <img :src="data.author.picture" />
            <span v-html="data.author.description"></span>
        </details>

    </div>
</template>

<script>
export default {
    name: 'BlogPost',
    props: ['post'],
    data() {
        return {
            data: {
                "id": '',
                "slug": '',
                "link": '',
                "permalink": '',
                "title": '',
                "headline": '',
                "excerpt": '',
                "featured_media": '',
                "categories": '',
                "options": '',
                "breadcrumbs": '',
                "content": '',
                "bibliography": '',
                "tags": '',
                "author": '',
                "published": '',
                "modified": '',
                "reviewed": '',
                "reviewed_by": '',
                "metas": '',
                "related_links": '',
                "previous_post": '',
                "next_post": '',
                "previous_posts": '',
                "next_posts": '',
                "sidebars": '',
                "parent": '',
                "summary": '',
                "hreflang": '',
            }
        }
    },
    methods: {
        fetchPost(id) {
            fetch(`https://api.beta.mejorconsalud.com/wp-json/mc/v1/posts/${id}`)
                .then(res => res.json())
                .then((res) => {
                    this.data = res;
                    console.log(this.data);
                })
                .catch((err) => console.log(err))
        }

    },
    created() {
        this.fetchPost(this.$route.params.id);
    },
}
</script>