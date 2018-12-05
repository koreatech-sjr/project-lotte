<template>
  <div>
    <lotte-index-category></lotte-index-category>
    <lotte-index-slide></lotte-index-slide>
    <lotte-index-menu></lotte-index-menu>
  </div>
</template>

<script>
  import MenuComponent from './Components/MenuComponent';
  import SlideComponent from './Components/SlideComponent';
  import CategoryComponent from './Components/CategoryComponent/CategoryComponent';


  export default {
    name: 'Index',
    components : {
      'lotte-index-menu': MenuComponent,
      'lotte-index-slide': SlideComponent,
      'lotte-index-category': CategoryComponent,
    },
    data () {
      return {

      }
    }
  }
</script>

<style scoped>
  * {

  }
</style>
