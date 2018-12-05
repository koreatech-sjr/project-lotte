<template>
  <div id="app">
    <lotte-topnav></lotte-topnav>
    <router-view/>
  </div>
</template>

<script>
  import TopNavComponent from './components/SharedComponent/TopNavComponent/TopNavComponent'

  export default {
    components: {
      "lotte-topnav": TopNavComponent
    },
    name: 'App'
  }
</script>

<style>
  #app {
    width: 100%;
    text-align: center;
  }
</style>
