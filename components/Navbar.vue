<!-- Please remove this file from your project -->
<template>
<div>
  <NuxtLink to="/">Home page</NuxtLink>

  <nuxt-link to="/login" class="button--green">
    Login
  </nuxt-link>
  <nuxt-link to="/articles" class="button--grey">
    Articles
  </nuxt-link>
</div>
</template>

<script>
export default {
  name: 'Navbar',
}
</script>
