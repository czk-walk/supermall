<template>
  <div class="cart">
    <!-- 导航 -->
    <nav-bar class="cart-nav-bar">
      <div slot="center">购物车({{cartlength}})</div>
    </nav-bar>

    <!-- 商品的列表 -->
    <cart-list></cart-list>

    <!-- 底部汇总 -->
    <cart-bottom-bar></cart-bottom-bar>
  </div>
</template>

<script>    
import NavBar from 'components/common/navbar/NavBar.vue'
import CartList from './childComps/CartList.vue';
import CartBottomBar from './childComps/CartBottomBar.vue';

import { mapGetters } from "vuex";

export default {
  name: "cart",
  components: { 
    NavBar,
    CartList,
    CartBottomBar, 
  },
  computed: {
    //两种语法
    ...mapGetters([
      'cartlength',
    ])

    //如果想取另外的名字，使用对象形式
    // ...mapGetters({
    //   haha: 'cartlength',
    // })

  }
}
</script>

<style>
.cart{
  height: 100vh;
}
.cart-nav-bar {
  background-color: var(--color-tint);
  color: #fff;
}
</style>