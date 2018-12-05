<template>
  <div class="container">
    <!--상단 카테고리 디스플레이-->
    <div
      v-if="!addFlag"
      class="category-box">
      <span
        :class="{selected : nowCategory === category}"
        @click="clickCategory(category)"
        v-for="category in myCategory"
        :key="category.id"
        class="category">
        {{ category }}
      </span>
    </div>

    <!--상단 카테고리 추가 및 변경부-->
    <div
      v-if="!addFlag"
      @click="clickAdd()"
      class="category-add">
      <img src="../../../../assets/add.png" alt="">
    </div>


    <!--나만의 카테고리 표시부-->
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
        nowCategory: "",
        addFlag: false
      }
    },
    methods: {
      clickAdd: function () {
        this.addFlag = !this.addFlag;
      },
      clickCategory: function(val) {
        this.nowCategory = val;
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
        this.nowCategory = this.myCategory[0];
      }
    },
    created() {
      this.myCategory = this.$localStorage.get("myCategory") === null || this.$localStorage.get("myCategory") === '[]' ? this.returnCategoryName(this.categorySample) : JSON.parse(this.$localStorage.get("myCategory"));
      this.nowCategory = this.myCategory[0];
    }
  }
</script>

<style scoped>
  .container {
    height: 35px;
    padding-top: 5px;
    margin-top: 5px;
    padding-bottom: 10px;
  }

  .category-box {
    float: left;
    width: 90%;
    height: 40px;
    white-space: nowrap;
    overflow-x: scroll;
    text-align: left;
  }

  .category-box .category {
    display: inline-block;
    margin-left: 10px;
    margin-right: 10px;
    height: 22px;
    font-weight: 700;
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

  .selected {
    color: #6f99ff;
    height: 10px;
    border-bottom: 2px #6f99ff solid;
  }
</style>
