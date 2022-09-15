<template>
    <article>
        <h3>{{post.title}}</h3>
        <p>{{post.body}}</p>
        <button @click="redirectPost">{{post.id}}</button>
    </article>
</template>

<script>
    export default{
        name:'PostView',
        props: {
            post: {
                type: [Object, Array],
                required: true
            }
        },
        methods:{
            redirectPost() {
                this.$router.push({ name: 'blogpost', params: { id: `${this.post.id}` } })
            }
        }
    }
</script>