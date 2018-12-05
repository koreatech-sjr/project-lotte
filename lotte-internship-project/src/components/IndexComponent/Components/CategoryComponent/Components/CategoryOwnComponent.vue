<template>
  <div class="container">
    <div class="header">
      내가 추가한 메뉴
      <span>
        {{ myCategory.length }}
      </span>
      <span
        @click="close"
        class="close">
        닫기
      </span>
      <span
        @click="changeOrder"
        class="order">
        순서변경
      </span>

    </div>

    <div class="category-badge-group">
      <span
        @click="clickMyCategory(category.dispNm)"
        class="category"
        :class="{'my' : myCategory.indexOf(category.dispNm) !== -1}"
        :id="category.id"
        v-for="category in categorySample">
        {{ category.dispNm }}
      </span>
    </div>

    <div class="header">
      전체 메뉴
      <span>
        {{ categorySample.length }}
      </span>
    </div>


    <div class="category-image-group">
      <div
        @click="clickMyCategory(category.dispNm)"
        class="category-image-box"
        :id="category.id"
        v-for="category in categorySample">
        <div class="image-container">
          <img :src="category.imgPath">
          <div class="category-desc">{{ category.dispNm }}</div>
        </div>
        <div class="image-container">
          <img
            class="check"
            v-if="myCategory.indexOf(category.dispNm) !== -1"
            src="../../../../../assets/check.png">
        </div>

      </div>
    </div>
    <div
      v-if="orderFlag">
      <lotte-index-category-order-change></lotte-index-category-order-change>
    </div>
  </div>
</template>

<script>
  import CategoryOrderChangeComponent from './CategoryOrderChangeComponent'

  export default {
    name: 'CateogryOwn',
    components: {
      "lotte-index-category-order-change": CategoryOrderChangeComponent,
    },
    props: {
      categorySample: null,
    },
    data() {
      return {
        myCategory: [],
        orderFlag: false
      }
    },
    methods: {
      clickMyCategory: function (name) {
        for (let i = 0; i < this.myCategory.length; i++) {
          if (this.myCategory[i] === name) {
            this.myCategory[i] = null;
            this.myCategory = this.removeNull(this.myCategory);
            this.$localStorage.set("myCategory", JSON.stringify(this.myCategory));
            console.log(this.myCategory);
            return
          }
        }

        this.myCategory.push(name);
        console.log(this.myCategory);
        this.$localStorage.set("myCategory", JSON.stringify(this.myCategory));
      },
      removeNull: function (array) {
        if (array === null) {
          return
        }
        let tmp = [];
        for (let i = 0; i < array.length; i++) {
          if (array[i] !== null) {
            tmp.push(array[i]);
          }
        }
        return tmp;
      },
      close: function () {
        this.$emit('close');
      },
      changeOrder: function () {
        this.orderFlag = !this.orderFlag;
      }
    },
    created() {
      this.myCategory = this.$localStorage.get("myCategory") === null ? [] : JSON.parse(this.$localStorage.get("myCategory"));
    }
  }
</script>

<style scoped>
  .container {
    position: relative;
    height: 100%;
    background: #6f99ff;
    z-index: 1;
  }

  .header {
    text-align: left;
    padding-left: 20px;
  }

  .category-badge-group, .category-image-group {
    padding-top: 20px;
    padding-bottom: 20px;
  }

  .category-image-group {
    height: 400px;
  }

  .category-badge-group .category {
    display: inline-block;
    font-size: 12px;
    margin-left: 5px;
    margin-right: 5px;
    padding-left: 10px;
    padding-right: 10px;
    border: black 1px solid;
    border-radius: 30px;
  }

  .category-image-group .category-image-box {
    width: 31%;
    margin: 2% 1%;
    height: 70px;
    float: left;
  }

  .image-container {
    width: 31%;
    margin-left: 1%;
    margin-right: 1%;

    height: 70px;
    position: absolute;
  }

  .category-image-group .category-image-box .image-container img {
    width: 50px;
    position: relative;
    z-index: 2;
  }

  .category-image-group .category-image-box .check {
    opacity: 0.5;
    position: relative;
    z-index: 3;
  }

  .category-image-group .category-image-box .category-desc {
    font-size: 13px;
  }

  .my {
    background: white;
  }

  .close, .order {
    cursor: pointer;
    float: right;
    margin-right: 10px;
  }
</style>
