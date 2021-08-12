<template>
  <div class="detail-swiper">
    <swiper class="swiperStyle">
      <swiper-item v-for="(item,index) in topImages" :key="index">
          <img :src="item" alt="">
      </swiper-item>
    </swiper>
  </div>
</template>

<script>
import Swiper from "components/common/swiper/Swiper.vue";
import SwiperItem from "components/common/swiper/SwiperItem.vue";
export default {
  components: { Swiper, SwiperItem },
  name: "DetailSwiper",
  props: {
    topImages: {
      type: Array
    },
  },
};
</script>

<style scoped>
    .swiperStyle{
        height: 400px;
        overflow: hidden;
    }
</style>