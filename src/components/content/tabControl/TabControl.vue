<template>
  <div id="TabControl">
    <div v-for="(item, index) in titles" :key="index" class="control-item" :class="{active:i===index}" @click="changeClass(index)">
      <span>{{ item }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "TabControl",
  data () {
      return {
          i:0
      }
  },
  props: {
    titles: {
      type: Array,
      default() {
        return [];
      },
    },
  },
  methods: {
      changeClass(index){
          this.i = index
          this.$emit('tabClick',index)
      }
  }
};
</script>

<style scoped>
#TabControl {
  display: flex;
  text-align: center;
  font-size: 15px;
  background-color: #fff;
 
}
.control-item {
  flex: 1;
  height: 40px;
  line-height: 40px;
}
.control-item span{
    padding: 5px;
}
.active{
    color: var(--color-high-text);
}
.active span{
    border-bottom: 3px solid var(--color-tint);
}
</style>