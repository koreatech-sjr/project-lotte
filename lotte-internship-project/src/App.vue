<template>
  <div id="app">
    <lotte-topnav></lotte-topnav>
    <router-view/>
  </div>
</template>

<script>
  import TopnavComponent from './components/SharedComponent/TopnavComponent/TopnavComponent'

  export default {
    components: {
      "lotte-topnav": TopnavComponent
    },
    name: 'App'
  }
</script>

<style>
  #app {
    text-align: center;
  }
</style>
