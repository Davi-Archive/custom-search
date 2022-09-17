<template>
    <article>
        <h3>{{post.title}}</h3>
        <p>{{post.body}}</p>
       <!--  <button @click="()=>fetchPost(post.id)"></button> -->
        <nuxt-link :to="{ path: 'post', query: { id: post.id }}"><button>{{post.id}}</button></nuxt-link>
        <button @click="()=>fetchPost(post.id)"></button>
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
    data(){
        return {
            data: '',
        }
    },
    methods: {
        fetchPost(id) {
            fetch(`https://api.beta.mejorconsalud.com/wp-json/mc/v1/posts/${id}`)
                .then(res => res.json())
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