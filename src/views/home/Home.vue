<template>
  <div id="home">
    <NavBar class="home-nav">
      <div slot="center">购物车</div>
    </NavBar>
    <scroller
      class="home-scroller"
      ref="scroller"
      :probe-type="3"
      @scroll="getPostion"
      :pullUpLoad="true"
      @pullingUp="LoadMore"
    >
      <home-swiper :cbanners="banners" />
      <home-recommend :crecommend="recommend" />
      <home-feature />
      <tab-control class="tabcontrol" :titles="titles" @tabClick="tabClick" />
      <goods-list :goods="showGoods" />
    </scroller>
    <!-- .native 监听原生事件 -->
    <back-top @click.native="backTopClick" v-show="isShowBackTop" />
  </div>
</template>

<script>
import NavBar from "components/common/navbar/NavBar.vue";

import { getHomeMultidata, getHomeGoods } from "network/home";

import HomeSwiper from "./childComps/HomeSwiper.vue";
import HomeRecommend from "./childComps/HomeRecommend.vue";
import HomeFeature from "./childComps/HomeFeature.vue";
import TabControl from "../../components/content/tabControl/TabControl.vue";
import GoodsList from "../../components/content/goods/GoodsList.vue";
import Scroller from "../../components/common/scroller/Scroller.vue";
import BackTop from "../../components/content/backTop/BackTop.vue";

export default {
  components: {
    NavBar,
    HomeSwiper,
    HomeRecommend,
    HomeFeature,
    TabControl,
    GoodsList,
    Scroller,
    BackTop,
  },
  name: "Home",
  data() {
    return {
      titles: ["流行", "新款", "精选"],
      banners: [],
      recommend: [],
      goods: {
        pop: { page: 0, list: [] },
        new: { page: 0, list: [] },
        sell: { page: 0, list: [] },
      },
      crruentType: "pop",
      isShowBackTop: false,
    };
  },
  created() {
    this.getHomeMultidata();
    this.getHomeGoods("pop");
    this.getHomeGoods("new");
    this.getHomeGoods("sell");
    
  },
  mounted () {
    //防抖
  const refresh = this.debounce(this.$refs.scroller.refresh,200)
    
    this.$bus.$on("imgload", () => {
      console.log("-----");
      // this.$refs.scroller && this.$refs.scroller.refresh();
      refresh()
    });
  },
  methods: {
    
    debounce(func,delay){
      let timer = null
      return function (...args) {
        if(timer) clearTimeout(timer)
      timer = setTimeout(() => {
        func.apply(this,args)
      }, delay);
      }
    },

    //网络请求相关方法
    getHomeMultidata() {
      getHomeMultidata().then((res) => {
        console.log(res.data);
        this.banners = res.data.banner.list;
        this.recommend = res.data.recommend.list;
      });
    },
    getHomeGoods(type) {
      const page = this.goods[type].page + 1;
      getHomeGoods(type, page).then((res) => {
        console.log(res);
        this.goods[type].list.push(...res.data.list);
        // console.log(this.goods[type].list);
        this.goods[type].page += 1;
        this.$refs.scroller.finishPullUp();
      });
    },

    //事件监听相关方法
    tabClick(index) {
      console.log(index);
      switch (index) {
        case 0:
          this.crruentType = "pop";
          break;
        case 1:
          this.crruentType = "new";
          break;
        case 2:
          this.crruentType = "sell";
          break;
      }
    },
    backTopClick() {
      // 通过$refs拿到组件中的对象
      this.$refs.scroller.scrollTo(0, 0, 500);
    },
    getPostion(postion) {
      this.isShowBackTop = -postion.y > 300;
    },
    LoadMore() {
      console.log("loading");
      this.getHomeGoods(this.crruentType);
    },
  },
  computed: {
    showGoods() {
      return this.goods[this.crruentType].list;
    },
  },
};
</script>

<style scoped>
#home {
  padding-top: 44px;
}
.home-nav {
  background-color: var(--color-tint);
  color: #fff;
  position: fixed;
  left: 0;
  right: 0;
  top: 0;
  z-index: 9;
}
.tabcontrol {
  position: sticky;
  top: 44px;
  z-index: 9;
}
.home-scroller {
  /* height:calc(100vh - 93px); */
  overflow: hidden;
  position: absolute;
  top: 44px;
  bottom: 49px;
  right: 0;
  left: 0;
}
</style>