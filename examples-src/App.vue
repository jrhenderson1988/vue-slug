<template>
  <div id="app">
    Hello World!

    <input type="text" v-model="name">

    <vue-slug :input="name" v-model="slug"></vue-slug>

    <div style="margin: 20px 0 0 0;">{{ name }}</div>
    <div>{{ slug }}</div>
  </div>
</template>

<script>
  import VueSlug from '../src/Slug.vue'

  export default {
    name: 'app',
    components: {
      VueSlug
    },
    data() {
      return {
        name: 'This is a test',
        slug: 'this-is-a-test'
      }
    },
    computed: {
    },
    mounted() {
    }
  }
</script>

<style>
  html, body {
    font-family: 'Open Sans', 'Helvetica Neue', Helvetica, Arial, sans-serif;
    font-size: 100%;
  }

  html {
    height: 100%;
  }

  body {
    overflow: auto;
    position: relative;
  }
</style>
