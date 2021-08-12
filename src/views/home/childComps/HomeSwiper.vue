<template>
  <div>
      <swiper>
      <swiper-item v-for="(item, index) in cbanners" :key="index">
        <a :href="item.link">
          <img :src="item.image" alt="" @load="hsimgLoad" />
        </a>
      </swiper-item>
    </swiper>
  </div>
</template>

<script>
import { Swiper, SwiperItem } from "components/common/swiper";
export default {
    name:"HomeSwiper",
    components: {
        Swiper,
        SwiperItem
    },
    data () {
      return {
          isLoad:false
      }
    },
    props:{
        cbanners:{
            type:Array,
            default(){
                return []
            }
        }

    },
    methods: {
      hsimgLoad(){
        if(!this.isLoad){
        this.$emit('SwiperImgLoad')
        this.isLoad = true
        }
      }
    } 
    }
</script>

<style>

</style>