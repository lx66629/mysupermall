<template>
  <div class="goods-list">
    <div class="goods-item" v-for="(item,index) in goods" :key="index">
      <img :src="item.image" alt="" />
      <div class="goods-info">
        <p>{{ item.title }}</p>
        <span class="price">￥{{ item.price }}</span>
        <span class="collect">{{ item.cfav }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "DetailRecommendsInfo",
  props: {
    goods: {
      type: Array,
      defalut() {
        return [];
      },
    },
  },
};
</script>

<style scoped>
.goods-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  background-color: var(--color-background);
  padding: 2px;
}
.goods-item {
  padding-bottom: 45px;
  position: relative;
  width: 48%;
  /* flex: 1; */
}

.goods-item img {
  width: 100%;
  border-radius: 5px;
  border: 1px solid var(--color-tint);
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
  content: "";
  position: absolute;
  left: -15px;
  top: -1px;
  width: 14px;
  height: 14px;
  background-color: #eee;
  background: url("~assets/img/common/collect.svg") 0 0/14px 14px;
}
</style>