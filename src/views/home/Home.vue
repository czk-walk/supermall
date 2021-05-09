<template>
  <div id="home" class="wrapper">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>

    <scroll
      class="content"
      ref="scroll"
      :probe-type="3"
      @scroll="contentScroll"
      :pull-up-load="true"
    >
      <home-swiper :banners="banners" />
      <recommend-view :recommends="recommends" />
      <feature-view />
      <tab-control
        class=".tab-control"
        :titles="['流行', '新款', '精选']"
        @tabClick="tabClick"
      />
      <goods-list :goods="showGood" />
    </scroll>

    <back-top @click.native="topClick" v-show="isShowBackTopL"></back-top>
  </div>
</template>

<script>
import HomeSwiper from "./childComps/HomeSwiper";
import RecommendView from "./childComps/RecommendView";
import FeatureView from "./childComps/FeatureView.vue";

import NavBar from "components/common/navbar/NavBar";
import TabControl from "../../components/content/tabControl/TabControl.vue";
import GoodsList from "../../components/content/goods/GoodsList.vue";
import Scroll from "../../components/common/scroll/Scroll.vue";
import BackTop from "../../components/content/backTop/BackTop.vue";

import { getHomeMultidata, getHomeGoods } from "network/home";

export default {
  name: "Home",
  components: {
    HomeSwiper,
    RecommendView,
    FeatureView,
    NavBar,
    TabControl,
    GoodsList,
    Scroll,
    BackTop,
  },
  data() {
    return {
      banners: [],
      recommends: [],
      goods: {
        pop: { page: 0, list: [] },
        new: { page: 0, list: [] },
        sell: { page: 0, list: [] },
      },
      currentType: "pop",
      click: true,
      isShowBackTopL: false
    };
  },
  computed: {
    showGood() {
      return this.goods[this.currentType].list;
    },
  },
  created() {
    //1.请求多个数据
    this.getHomeMultidata();

    //2.请求商品数据
    this.getHomeGoods("pop");
    this.getHomeGoods("new");
    this.getHomeGoods("sell");

  },
  mounted() {
    //3.监听item中图片加载完成
    this.$bus.$on('itemImageLoad', () => {
      this.$refs.scroll.refresh();
      console.log('llll')
    })
  },
  methods: {
    tabClick(index) {
      // switch(index) {
      //   case 0:
      //     this.currentType = 'pop';
      //     break;
      //   case 1:
      //     this.currentType = 'new';
      //     break;
      //   case 2:
      //     this.currentType = 'sell';
      //     break;
      // }
      if (index == 0) {
        this.currentType = "pop";
      } else if (index == 1) {
        this.currentType = "new";
      } else {
        this.currentType = "sell";
      }
    },
    topClick() {
      this.$refs.scroll.scrollTo(0, 0, 500);
    },
    contentScroll(position) {
      this.isShowBackTopL = -position.y > 1000;
    },

    getHomeMultidata() {
      getHomeMultidata().then((res) => {
        (this.banners = res.data.banner.list),
          (this.recommends = res.data.recommend.list);
      });
    },
    getHomeGoods(type) {
      const page = this.goods[type].page + 1;
      getHomeGoods(type, page).then((res) => {
        this.goods[type].list.push(...res.data.list);
        this.goods[type].page += 1;
      });
    },
  },
};
</script>

<style scoped>
#home {
  padding-top: 44px;
  height: 100vh;
  position: relative;
}
.home-nav {
  background-color: var(--color-tint);
  color: aliceblue;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 9;
}

.tab-control {
  position: sticky;
  top: 44px;
  z-index: 9;
}

/* .content {
  height: calc(100% - 93px);
  overflow: hidden;
  margin-top: 51px;
} */

.content {
  overflow: hidden;
  position: absolute;
  top: 44px;
  bottom: 49px;
}
</style>