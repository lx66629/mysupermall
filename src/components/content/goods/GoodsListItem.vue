<template>
<div class="goods-item" @click="clickItem">
    <img :src="goodsItem.show.img" alt="" @load="ImgLoad">
    <div class="goods-info">
        <p>{{goodsItem.title}}</p>
        <span class="price">￥{{goodsItem.price}}</span>
        <span class="collect">{{goodsItem.cfav}}</span>
    </div>
</div>
</template>

<script>
export default {
    name:'GoodsListItem',
    props: {
        goodsItem:{
            type:Object
        }
    },
    methods: {
      ImgLoad(){
        this.$bus.$emit('imgload')
      },
      clickItem(){
        console.log('---商品被点击了---');
        this.$router.push({
          path:'/detail',
          query:{
            iid:this.goodsItem.iid
          }
        })
      }
    }
}
</script>

<style scoped>
  .goods-item {
    padding-bottom: 45px;
    position: relative;
    width: 48%;
    /* flex: 1; */
  }

  .goods-item img {
    width: 100%;
    border-radius: 5px;
    border:1px solid var(--color-tint);
  }

  .goods-info {
    font-size: 12px;
    position: absolute;
    bottom: 10px;
    left: 0;
    right: 0;
    overflow: hidden;
    text-align: center;
  }

  .goods-info p {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    margin-bottom: 3px;
  }

  .goods-info .price {
    color: var(--color-high-text);
    margin-right: 35px;
  }

  .goods-info .collect {
    position: relative;
  }

  .goods-info .collect::before {
    content: '';
    position: absolute;
    left: -15px;
    top: -1px;
    width: 14px;
    height: 14px;
    background-color:#eee;
    background: url("~assets/img/common/collect.svg") 0 0/14px 14px;
  }

</style>