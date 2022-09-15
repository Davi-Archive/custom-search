<template>
  <div class="about">
    <h1>This is an about page</h1>
    <button @click="redirectPost">post</button>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {},
  methods: {
    redirectPost(){
      this.$router.push({ name: 'BlogPost', params: { id: 'davi' } })
    }
  }
}
</script>
