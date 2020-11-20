<template>
  <div id="q-app">
    <Preloader></Preloader>
    <router-view />
  </div>
</template>
<script>
import Preloader from 'components/Preloader.vue'
export default {
  name: 'App',
  components:{Preloader}
}
</script>
<style>
body{
  background: url(./assets/sentry.png), linear-gradient(to bottom, #283c86, #45a247);
}
</style>
