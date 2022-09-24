<template>
    <div
        class='relative overflow-hidden flex flex-col justify-between space-y-3 text-sm rounded-xl min-w-[20rem] max-w-[23rem] p-4 mb-4 bg-white text-black dark:bg-slate-800 dark:text-white'>
        <div class='flex items-center justify-between font-medium'>
        </div>
        <div class='flex flex-row items-center space-x-3'>
            <div class='flex flex-none items-center justify-center w-21 h-21 rounded-full bg-green-500 text-white'>
                <div class='flex flex-none items-center justify-center w-16 h-16 rounded-full bg-green-500'>
                    <picture>
                        <img :src="buildImageUrl(img)" alt="" />
                    </picture>
                </div>
            </div>
            <span class='text-base font-medium'>{{titulo}}</span>
        </div>
        <div v-html="texto"> </div>
        <div class='flex justify-between items-center'>
            <div>
                <dt class='sr-only'>Users</dt>
                <dd class='flex justify-start -space-x-1.5'>
                    <span v-for="item in categories" class="text-gray-600 text-sm ">
                        <a :href="item.link">{{ item.name }}</a>
                    </span>
                </dd>
            </div>
            <a :href="`/post?id=${id}`">
                <button
                    class='flex items-center justify-center text-xs font-medium rounded-full px-4 py-1 space-x-1 border-2 border-black bg-white hover:bg-black hover:text-white text-black dark:bg-slate-800 dark:text-white dark:border-white dark:hover:bg-white dark:hover:text-black'>
                    <span>Ver mais</span>
                    <svg xmlns='http://www.w3.org/2000/svg' width='20' height='20' viewBox='0 0 24 24' fill='none'
                        stroke='currentColor' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'>
                        <path d='M5 12h13M12 5l7 7-7 7' />
                    </svg>
                </button>
            </a>
        </div>
    </div>
</template>



<script>
export default {

    name: 'PostLeftBar',
    props: [
        'id', 'categories', 'titulo', 'texto', 'img',
    ],
    methods: {
        buildImageUrl(image) {
            if (!image) return "https://raw.githubusercontent.com/davi38/custom-search/master/static/placeholder.webp";
            return `${image}`
        },
    }
}
</script>