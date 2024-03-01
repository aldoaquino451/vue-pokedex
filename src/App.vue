<script>
import axios from 'axios';
import IntroLayer from './components/IntroLayer.vue';
import Header from './components/Header.vue';
import Main from './components/Main.vue';

export default {
  components: {
    IntroLayer,
    Header,
    Main
  },

}

</script>

<template>

  <IntroLayer />

  <Header />

  <Main />

</template>

<style lang="scss">


</style>
