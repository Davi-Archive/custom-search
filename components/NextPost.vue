<template>
    <div class="inline-flex max-w-lg mx-auto justify-center">
        <div class="bg-white shadow-md border border-gray-200 rounded-lg max-w-sm mb-5">
            <a href="#">
                <img class="rounded-t-lg" :src="img" :alt="data.title">
            </a>
            <div class="p-5">
                <a href="#">
                    <h5 class="text-gray-900 font-bold text-2xl tracking-tight mb-2">{{data.title}}
                    </h5>
                </a>
                <p class="font-normal text-gray-700 mb-3" v-html="data.excerpt"></p>
                <a class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-3 py-2 text-center inline-flex items-center"
                    :href="`/post?id=${data.id}`">
                   Leia Mais
                </a>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    name: 'NextPost',
    props: ['data', 'img']
}
</script>