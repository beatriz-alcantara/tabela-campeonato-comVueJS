<template>
  <div id="app">
    <Tabela/>
  </div>
</template>

<script>
import Tabela from './components/Tabela.vue'

export default {
  name: 'app',
  components: {
    Tabela
  }
}
</script>

<style>
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
