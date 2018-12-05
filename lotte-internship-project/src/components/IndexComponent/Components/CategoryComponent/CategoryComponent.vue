<template>
  <div>
    <lotte-index-category-own></lotte-index-category-own>
    <lotte-index-category-order-change></lotte-index-category-order-change>
    {{ msg }}
  </div>
</template>

<script>
  import CategoryOrderChangeComponent from './Components/CategoryOrderChangeComponent'
  import CategoryOwnComponent from './Components/CategoryOwnComponent'

  export default {
    name: 'Category',
    components: {
      "lotte-index-category-order-change": CategoryOrderChangeComponent,
      "lotte-index-category-own": CategoryOwnComponent
    },
    data () {
      return {
        msg: 'Category'
      }
    }
  }
</script>

<style scoped>
</style>
