<template>
  <div class="container">

    <div class="header">
      <img
        class="menu"
        src="../../../assets/menu.png">
      <div
        @click="clickSideNav"
        class="contents">
        Lotte SpecTackle Project
      </div>
    </div>

    <div class="search">
      <div class="search-box">
        <input type="text">
        <div
          @click="clickSearch"
          class="search-btn">search</div>
      </div>
      <div
        @click="clickCart()"
        class="cart">
        <img
          class="logo"
          src="../../../assets/cart.png" >
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'TopNav',
    data() {
      return {
        msg: 'topnav'
      }
    },
    methods: {
      clickSideNav: function() {
        alert("준비중입니다!");
      },
      clickSearch: function() {
        alert("준비중입니다!");
      },
      clickCart: function() {
        alert("준비중입니다!");
      }
    }
  }
</script>

<style scoped>
  .container {
    width: calc(100% - 40px);
    height: 50px;
    padding: 20px 20px;
    text-align: left;
    background: #6f99ff;
  }
  .header{
    height: 30px;
  }

  .header .menu {
    cursor: pointer;
    float: left;
    width: 20px;
    height: auto;
  }
  .header .contents {
    padding-left: 20px;
    font-size: 18px;
    font-weight: 700;
    float: left;
    color: white;
  }
  .search {
    height: 30px;
    color: white;
  }

  .search .search-box {
    width: calc(100% - 20px);
    float: left;
  }

  .search .search-box input {
    float: left;
    width: 75%;
    height: 24px;
    border-radius: 10px;
    border: none;
  }

  .search .search-box .search-btn {
    position: relative;
    top: 4px;
    cursor: pointer;
    float: left;
    padding-left: 10px;
    width: calc(25% - 30px);
  }
  .search .cart {
    float: left;
  }
  .search .cart .logo{
    width: 20px;
  }

</style>
