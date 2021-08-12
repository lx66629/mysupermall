<template>
  <div id="detail">
    <detail-nav-bar class="detail-nav" @changetitle="changetitle" ref="nav" />
    <scroller
      class="content"
      ref="scroller"
      :probe-type="3"
      :pullUpLoad="true"
      @scroll="contentscroll"
      @pullingUp="LoadMore"
    >
      <detail-swiper :topImages="topImages" />
      <detail-base-info :goods="goods" />
      <detail-shop-info :shop="shop" />
      <detail-goods-info :detailInfo="detailInfo" @imgLoad="imgLoad" />
      <detail-param-info :paramInfo="paramInfo" ref="param" />
      <detail-comment-info :commentInfo="commentInfo" ref="comment" />
      <detail-recommends-info :goods="recommends" ref="recommends" />
    </scroller>
  </div>
</template>

<script>
import {
  getDetail,
  Goods,
  Shop,
  GoodsParam,
  getRecommend,
} from "network/detail";
import DetailNavBar from "./childComps/DetailNavBar.vue";
import DetailSwiper from "./childComps/DetailSwiper.vue";
import DetailBaseInfo from "./childComps/DetailBaseInfo.vue";
import DetailShopInfo from "./childComps/DetailShopInfo.vue";
import Scroller from "../../components/common/scroller/Scroller.vue";
import DetailGoodsInfo from "./childComps/DetailGoodsInfo.vue";
import DetailParamInfo from "./childComps/DetailParamInfo.vue";
import DetailCommentInfo from "./childComps/DetailCommentInfo.vue";
import DetailRecommendsInfo from "./childComps/DetailRecommendsInfo.vue";
export default {
  name: "Detail",
  components: {
    DetailNavBar,
    DetailSwiper,
    DetailBaseInfo,
    DetailShopInfo,
    Scroller,
    DetailGoodsInfo,
    DetailParamInfo,
    DetailCommentInfo,
    DetailRecommendsInfo,
  },

  data() {
    return {
      iid: null,
      topImages: [],
      goods: {},
      shop: {},
      detailInfo: {},
      paramInfo: {},
      commentInfo: {},
      recommends: [],
      themeTopYs: [],
      currentIndex:0
    };
  },
  created() {
    //保存存入id
    this.iid = this.$route.query.iid;

    //根据id请求详情数据
    getDetail(this.iid).then((res) => {
      //   console.log(res);
      this.topImages = res.result.itemInfo.topImages;

      //获取商品信息
      this.goods = new Goods(
        res.result.itemInfo,
        res.result.columns,
        res.result.shopInfo.services
      );

      //获取店铺信息
      this.shop = new Shop(res.result.shopInfo);
      console.log(this.shop);

      //保存商品详情数据
      this.detailInfo = res.result.detailInfo;

      //获取商品参数
      this.paramInfo = new GoodsParam(
        res.result.itemParams.info,
        res.result.itemParams.rule
      );
      console.log(this.paramInfo);

      //获取评论信息
      if (res.result.rate.cRate !== 0) {
        this.commentInfo = res.result.rate.list[0];
      }
    });
    getRecommend().then((res) => {
      console.log(res);
      this.recommends = res.data.list;
    });
  },
  methods: {
    LoadMore() {
      console.log("loading");
    },
    imgLoad() {
      this.themeTopYs.push(0);
      this.themeTopYs.push(this.$refs.param.$el.offsetTop - 44);
      this.themeTopYs.push(this.$refs.comment.$el.offsetTop - 44);
      this.themeTopYs.push(this.$refs.recommends.$el.offsetTop - 44);
      console.log(this.themeTopYs);
      this.$refs.scroller.refresh();
    },
    changetitle(index) {
      console.log(index);
      this.$refs.scroller.scroller.scrollTo(0, -this.themeTopYs[index], 300);
    },
    contentscroll(position){
      // console.log(position);
      let positionY = -position.y;
      let length = this.themeTopYs.length;
      for(let i = 0;i<length;i++){
        if(this.currentIndex !== i && ((i < length - 1 && positionY >= this.themeTopYs[i] && positionY < this.themeTopYs[i+1]) ||(i === length - 1 && positionY>= this.themeTopYs[i]))){
          this.currentIndex = i;
          console.log(this.currentIndex);
          this.$refs.nav.currentindex = this.currentIndex
        }
      }
    }
  },
};
</script>

<style scoped>
#detail {
  height: 100vh;
  position: relative;
  z-index: 9;
  background-color: #fff;
}
.content {
  height: calc(100% - 44px);
}
.detail-nav {
  position: relative;
  z-index: 9;
  background-color: #fff;
}
</style>