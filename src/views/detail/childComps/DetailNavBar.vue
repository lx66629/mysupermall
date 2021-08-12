<template>
  <div id="DetailNavBar">
    <nav-bar>
        <div slot="left" @click="backClick">
            <img src="~assets/img/common/back.svg" alt="" class="leftback">
        </div>
      <div slot="center" class="title">
        <div
          v-for="(item, index) in title"
          :key="index"
          class="title-item"
          :class="{ active: index === currentindex }"
          @click="changetitle(index)"
        >
          {{ item }}
        </div>
      </div>
    </nav-bar>
  </div>
</template>

<script>
import NavBar from "components/common/navbar/NavBar.vue";
export default {
  components: { NavBar },
  name: "DetailNavBar",
  data() {
    return {
      title: ["商品", "参数", "评论", "推荐"],
      currentindex: 0,
    }
  },
  methods: {
      changetitle(index){
          this.currentindex = index
          this.$emit('changetitle',index)
      },
      backClick(){
        //   this.$router.go(-1)
        this.$router.back()
      }
  }
};
</script>

<style scoped>
.title {
  display: flex;
  font-size: 14px;
}
.title-item {
  flex: 1;
}
.active{
    color:var(--color-high-text);
}
.leftback{
    margin-top: 11px;
}
</style>