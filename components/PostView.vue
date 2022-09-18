<template>
    <article>
        <nuxt-link :to="{ path: 'post', query: { id: post.id }}">
            <div @click="()=>fetchPost(post.id)">
                <h3>{{post.title}}</h3>
                <p class="text-zinc-800 text-xs" v-html="post.excerpt"></p>
            </div>
        </nuxt-link>
    </article>
</template>

<script>
export default {
    name: 'PostView',
    props: {
        post: {
            type: [Object, Array],
            required: true
        }
    },
    data() {
        return {
            data: '',
        }
    },
    methods: {
        fetchPost(id) {
            this.$axios.$get(`https://api.beta.mejorconsalud.com/wp-json/mc/v1/posts/${id}`)
                .then((res) => {
                    this.data = res;
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
                    console.log(this.data, this.newDate);
                    redirectPost(this.data)
                })
                .catch((err) => console.log(err))
        },
        redirectPost(data) {
            this.$router.push({ name: 'post', params: { id: data } })
        }
    }
}
</script>