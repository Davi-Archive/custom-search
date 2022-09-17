<!-- Please remove this file from your project -->
<template>
  <nav class="w-full py-4 bg-blue-800 shadow">
    <div class="w-full container mx-auto flex flex-wrap items-center justify-between">

      <nav>
        <ul class="flex items-center justify-between font-bold text-sm text-white uppercase no-underline">
          <li>
            <NuxtLink to="/">Home page</NuxtLink>
          </li>
          <li>
            <nuxt-link to="/post?id=688159" class="button--green">
              Post Blog
            </nuxt-link>
          </li>
          <li><nuxt-link to="/about" class="button--green">
            About
          </nuxt-link></li>
        </ul>
      </nav>

      <div class="flex items-center text-lg no-underline text-white pr-6">
        <a class="" href="#">
          <i class="fab fa-facebook"></i>
        </a>
        <a class="pl-6" href="#">
          <i class="fab fa-instagram"></i>
        </a>
        <a class="pl-6" href="#">
          <i class="fab fa-twitter"></i>
        </a>
        <a class="pl-6" href="#">
          <i class="fab fa-linkedin"></i>
        </a>
      </div>
    </div>

  </nav>

  <!-- <div>
  <NuxtLink to="/">Home page</NuxtLink>

  <nuxt-link to="/post?id=688159" class="button--green">
    Post Blog
  </nuxt-link>
  <nuxt-link to="/articles" class="button--grey">
    Articles
  </nuxt-link>
</div> -->
</template>

<script>
export default {
  name: 'Navbar',
}
</script>
