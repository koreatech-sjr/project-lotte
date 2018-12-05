<template>
  <div class="container">
    <div
      v-if="!addFlag"
      class="category-box">
      <span
        v-for="category in myCategory"
        :key="category.id"
        class="category">
        {{ category }}
      </span>
    </div>
    <div
      v-if="!addFlag"
      @click="clickAdd()"
      class="category-add">
      <img src="../../../../assets/add.png" alt="">
    </div>

    <div v-if="addFlag">
      <lotte-index-category-own
        @close="close"
        :my-category="myCategory"
        :categorySample="categorySample"></lotte-index-category-own>
    </div>

  </div>
</template>

<script>
  import CategoryOwnComponent from './Components/CategoryOwnComponent'
  import CategorySample from '../../../../static/CategorySample';

  export default {
    name: 'Category',
    components: {
      "lotte-index-category-own": CategoryOwnComponent
    },
    data() {
      return {
        msg: 'Category',
        categorySample: CategorySample.data.items,
        myCategory: [],
        addFlag: false
      }
    },
    methods: {
      clickAdd: function () {
        this.addFlag = !this.addFlag;
      },
      returnCategoryName: function (ar) {
        let tmp = [];
        for (let i = 0; i < ar.length; i++) {
          tmp.push(ar[i].dispNm);
        }
        return tmp;
      },
      close: function () {
        this.addFlag = false;
        if(this.$localStorage.get("myCategory") === '[]'){
          this.myCategory = this.returnCategoryName(this.categorySample);
          return;
        }
        this.myCategory = JSON.parse(this.$localStorage.get("myCategory"));
      }
    },
    created() {
      this.myCategory = this.$localStorage.get("myCategory") === null || this.$localStorage.get("myCategory") === '[]' ? this.returnCategoryName(this.categorySample) : JSON.parse(this.$localStorage.get("myCategory"));
    }
  }
</script>

<style scoped>
  .container {
    height: 35px;
    padding-top: 10px;
    padding-bottom: 10px;
  }

  .category-box {
    float: left;
    width: 90%;
    height: 35px;
    white-space: nowrap;
    overflow-x: scroll;
  }

  .category-box .category {
    display: inline-block;
    padding-left: 10px;
    padding-right: 10px;
    height: 35px;
  }

  .category-add {
    cursor: pointer;
    float: left;
    width: 10%;
  }

  .category-add img {
    width: 20px;
    position: relative;
    bottom: 2px;
  }

  ::-webkit-scrollbar {
    width: 0px; /* remove scrollbar space */
    background: transparent; /* optional: just make scrollbar invisible */
  }

  /* optional: show position indicator in red */
  ::-webkit-scrollbar-thumb {
    background: #FFFFFF;
  }
</style>
