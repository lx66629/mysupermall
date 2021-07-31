<template>
  <div id="home">
    <NavBar class="home-nav">
      <div slot="center">购物车</div>
    </NavBar>
    <swiper>
      <swiper-item v-for="(item, index) in banners" :key="index">
        <a :href="item.link">
          <img :src="item.image" alt="" />
        </a>
      </swiper-item>
    </swiper>
  </div>
</template>

<script>
import NavBar from "components/common/navbar/NavBar.vue";
import { getHomeMultidata } from "network/home";
import { Swiper, SwiperItem } from "components/common/swiper";
export default {
  components: { NavBar, Swiper, SwiperItem },
  name: "Home",
  data() {
    return {
      banners: [],
      recommend: [],
    };
  },
  created() {
    getHomeMultidata().then((res) => {
      console.log(res.data);
      this.banners = res.data.banner.list;
      this.recommend = res.data.recommend;
    });
  },
};
</script>

<style scoped>
.home-nav {
  background-color: var(--color-tint);
  color: #fff;
}
</style>