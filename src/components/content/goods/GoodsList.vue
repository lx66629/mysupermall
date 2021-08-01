<template>
    <div class="goods-list">
        <goods-list-item v-for="(item,index) in goods" :key="index" :goodsItem="item" />
    </div>
</template>

<script>
import GoodsListItem from './GoodsListItem.vue'
export default {
  components: { 
      GoodsListItem 
      },
    name:'GoodsList',
    props: {
        goods:{
            type:Array,
            defalut(){
                return []
            }
        }
    }
}
</script>

<style scoped>
  .goods-list {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    background-color: var(--color-background);
    padding: 2px;
  }

</style>