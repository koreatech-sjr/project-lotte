<template>
  <div class="container">
    <!--내가 추가한 메뉴 상단 header-->
    <div class="header">
      내가 추가한 메뉴
      <span>
        {{ myCategory.length }}
      </span>
      <span
        @click="close"
        class="close">
        <img src="../../../../../assets/x.png">
      </span>
      <span
        @click="clickChangeOrder"
        class="order">
        순서변경
      </span>
    </div>


    <!--내가 추가한 메뉴 출력부-->
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


    <!--전체 메뉴 상단 header -->
    <div class="header">
      전체 메뉴
      <span>
        {{ categorySample.length }}
      </span>
    </div>
    <!--전체 메뉴 출력부 -->
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
    <!-- 변경 누를 경우 모달 생성 -->
    <modal
      style="z-index: 2"
      width="300"
      height="auto"
      name="dialog">

      <!--modal 내용부 -->
      <div class="modal-title">
        꾹 누른 후 드래그하셔서 순서를 변경해주세요!
      </div>
      <!--draggable을 이용해 순서 변경 -->
      <draggable class="list-group" element="ul" v-model="list" :options="dragOptions" :move="onMove"
                 @start="isDragging=true" @end="isDragging=false">
        <transition-group type="transition" :name="'flip-list'">
          <div class="list-group-item" v-for="element in list" :key="element.order">
            <i :class="element.fixed? 'fa fa-anchor' : 'glyphicon glyphicon-pushpin'"
               @click=" element.fixed=! element.fixed" aria-hidden="true"></i>
            {{element.dispNm}}
          </div>
        </transition-group>
      </draggable>
      <!--modal 내부 버튼부-->
      <div class="btn-group">
        <button
          class="change-done"
          @click="changeDone">변경하기
        </button>
        <button
          class="change-cancel"
          @click="cancel">취소하기
        </button>
      </div>
    </modal>
  </div>
</template>

<script>
  import draggable from "vuedraggable";

  export default {
    name: 'CategoryOwn',
    components: {
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
      // 카테고리 선택으로 로컬 스토리지에 추가 및 제거 하는 부분
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
      // 나의 카테고리 부분을 나갈 때 상위 컴포넌트로 나간다는 사실을 알리는 부분
      close: function () {
        this.$emit('close');
      },
      // 순서 변경 버튼을 누르는 부분
      clickChangeOrder: function () {
        //this.orderFlag = !this.orderFlag;
        this.list = JSON.parse(this.$localStorage.get("myCategory")).map((dispNm, index) => {
          return {dispNm, order: index + 1, fixed: false};
        })
        this.$modal.show('dialog');
      },

      //draggable로 item이 움직이는 부분
      onMove({relatedContext, draggedContext}) {
        const relatedElement = relatedContext.element;
        const draggedElement = draggedContext.element;
        return (
          (!relatedElement || !relatedElement.fixed) && !draggedElement.fixed
        );
      },


      // 변경완료 누른 경우
      changeDone: function () {
        this.myCategory = this.returnCategoryName(this.list);
        this.$localStorage.set("myCategory", JSON.stringify(this.myCategory));
        alert("나의 카테고리 순서가 변경되었습니다.");
        this.$modal.hide('dialog');
      },

      // 취소
      cancel: function () {
        this.$modal.hide('dialog');
      },

      //Utill용 list의 dispNm만 빼오는 메소드
      returnCategoryName: function (ar) {
        let tmp = [];
        for (let i = 0; i < ar.length; i++) {
          tmp.push(ar[i].dispNm);
        }
        return tmp;
      },
      //util용 array에서 null제거하고 값있는것만 배열로 받는 메소드
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
    },
    computed: {
      dragOptions() {
        return {
          animation: 0,
          group: "description",
          disabled: !this.editable,
          ghostClass: "ghost"
        };
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
    background: #f1f1f1;
    z-index: 0;
    margin-top: -5px;
  }

  .header {
    text-align: left;
    padding-left: 20px;
    font-size: 17px;
    font-weight: 700;
  }

  .category {
    background: white;
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
    background: white;
    opacity: 0.3;
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
    background: #6f99ff;
    color: black;
  }

  .close, .order {
    cursor: pointer;
    float: right;
    margin-right: 10px;
    font-size: 13px;
    font-weight: 400;
    padding: 2px 10px;
  }

  .close {
    padding-left: 0px;
  }

  .close img {
    width: 15px;

  }

  .order {
    border: #FFF solid 1px;
    border-radius: 5px;
  }

  .modal-title {
    padding: 20px;
  }

  .list-group {
    min-height: 20px;
    padding: 20px;
    margin: 0;
  }

  .list-group-item {
    color: #333;
    background-color: #fff;
    border: 1px solid #ccc;
    border-radius: 10px;
    margin-bottom: 2px;
    padding: 6px 12px;
    font-size: 14px;
    font-weight: 400;
    line-height: 1.42857143;
    text-align: left;
    cursor: move;
  }

  .list-group-item:hover, .list-group-item:focus {
    background: #9db8fb;
  }

  .list-group-item i {
    cursor: pointer;
  }

  .btn-group {
    text-align: center;
    padding: 0px 20px 20px;
  }

  .change-done {
    color: #333;
    padding: 6px 12px;
    margin-bottom: 0;
    font-size: 14px;
    font-weight: 400;
    line-height: 1.42857143;
    cursor: pointer;
    background: #9db8fb;
    border-radius: 5px;
    border: none;
  }

  .change-cancel {
    color: #333;
    background-color: #fb9d9d;
    padding: 6px 12px;
    margin-bottom: 0;
    font-size: 14px;
    font-weight: 400;
    line-height: 1.42857143;
    border-radius: 5px;
    cursor: pointer;
    border: none;
  }

</style>
