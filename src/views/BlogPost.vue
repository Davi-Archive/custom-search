<!-- eslint-disable vue/no-v-html -->
<template>
    <div class="blogPost">
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

        <body>
            <br />
            <span v-html="data.content"></span>
        </body>

    </div>
</template>

<script>


export default {
    name: 'BlogPost',
    data() {
        return {
            data: [],
            newDate: ''
        }
    },
    created() {
        this.fetchPost(this.$route.params.id);
        this.data = [
            {
                categories: { link: '1' }
            },
            {
                author: {
                    link: './',
                    name: '1'
                }
            }
        ]
    },
    methods: {
        fetchPost(id) {
            fetch(`https://api.beta.mejorconsalud.com/wp-json/mc/v1/posts/${id}`)
                .then(res => res.json())
                .then((res) => {
                    this.data = res;
                    console.log(this.data, this.newDate);


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