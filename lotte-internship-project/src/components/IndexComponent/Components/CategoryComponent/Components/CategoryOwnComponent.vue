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

    <modal
      style="z-index: 2"
      name="dialog">

      <draggable class="list-group" element="ul" v-model="list" :options="dragOptions" :move="onMove" @start="isDragging=true" @end="isDragging=false">
        <transition-group type="transition" :name="'flip-list'">
          <li class="list-group-item" v-for="element in list" :key="element.order">
            <i :class="element.fixed? 'fa fa-anchor' : 'glyphicon glyphicon-pushpin'" @click=" element.fixed=! element.fixed" aria-hidden="true"></i>
            {{element.dispNm}}
          </li>
        </transition-group>
      </draggable>

      <button @click="changeDone">변경하기</button>
      <button @click="cancel">취소하기</button>
    </modal>


  </div>
</template>

<script>
  import CategoryOrderChangeComponent from './CategoryOrderChangeComponent'
  import draggable from "vuedraggable";

  export default {
    name: 'CateogryOwn',
    components: {
      "lotte-index-category-order-change": CategoryOrderChangeComponent,
      draggable
    },
    props: {
      categorySample: null,
    },
    data() {
      return {
        myCategory: [],
        orderFlag: false,
        list: null,
        editable: true,
        isDragging: false,
        delayedDragging: false
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
        //this.orderFlag = !this.orderFlag;
        this.list = JSON.parse(this.$localStorage.get("myCategory")).map((dispNm, index) => {
          return { dispNm, order: index + 1, fixed: false };
        })
        this.$modal.show('dialog');
      },

      orderList() {
        this.list = this.list.sort((one, two) => {
          return one.order - two.order;
        });
      },
      onMove({ relatedContext, draggedContext }) {
        const relatedElement = relatedContext.element;
        const draggedElement = draggedContext.element;
        return (
          (!relatedElement || !relatedElement.fixed) && !draggedElement.fixed
        );
      },
      returnCategoryName: function (ar) {
        let tmp = [];
        for (let i = 0; i < ar.length; i++) {
          tmp.push(ar[i].dispNm);
        }
        return tmp;
      },
      changeDone: function() {
        this.myCategory = this.returnCategoryName(this.list);
        this.$localStorage.set("myCategory", JSON.stringify(this.myCategory));
        alert("나의 카테고리 순서가 변경되었습니다.");
        this.$modal.hide('dialog');
      },

      cancel: function() {
        alert("취소되었습니다.");
        this.$modal.hide('dialog');
      }
    },

    computed: {
      dragOptions() {
        return {
          animation: 0,
          group: "description",
          disabled: !this.editable,
          ghostClass: "ghost"
        };
      },
      listString() {
        return JSON.stringify(this.list, null, 2);
      },
      list2String() {
        return JSON.stringify(this.list2, null, 2);
      }
    },
    watch: {
      isDragging(newValue) {
        if (newValue) {
          this.delayedDragging = true;
          return;
        }
        this.$nextTick(() => {
          this.delayedDragging = false;
        });
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
    z-index: 0;
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
    opacity: 0.8;
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

  .order-list {
    width: 100vh;
    height: 100vh;
    background: black;
    z-index: 1;
  }
</style>
