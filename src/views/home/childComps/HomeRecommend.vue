<template>
  <div id="HomeRecommend">
      <div class="recommend-item" v-for="(item,index) in crecommend" :key="index">
          <a :href="item.link">
              <img :src="item.image" alt="">
              <div>{{item.title}}</div>
          </a>
      </div>
  </div>
</template>

<script>
export default {
    name:'HomeRecommend',
    props:{
        crecommend:{
            type:Array,
            default(){
                return []
            }
        }
    }
}
</script>

<style scoped>
    #HomeRecommend{
        display: flex;
        width: 100%;
        text-align: center;
        font-size: 14px;
        padding: 15px 0 20px 0;
        border-bottom: 8px solid #eee;
        background-color: #fff;
    }
    .recommend-item{
        flex:1 
    }
     .recommend-item img{
         width: 68px;
        height: 68px;
        margin-bottom: 8px;
     }
</style>