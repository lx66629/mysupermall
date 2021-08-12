<template>
  <div ref="wrapper">
    <div>
      <slot></slot>
    </div>
  </div>
</template>

<script>
    import BScroll from 'better-scroll'

    export default {
        name: "scroller",
        data() {
            return {
                scroller:null
            }
        },
        //接收父组件来的值
        props:{
            probeType:{
                type:Number,
                default:0
            },
            pullUpLoad:{
                type:Boolean,
                default:false
            }
        },
        mounted() {
            this.scroller = new BScroll(this.$refs.wrapper, {
                observeDOM: true,
                click:true,
                probeType:this.probeType,
                pullUpLoad:this.pullUpLoad
            })
            this.scroller.on('refresh', () => {

            })
            //监听滚动位置
            this.scroller.on('scroll', (postion) => {
                // console.log(postion);
                this.$emit('scroll', postion)
            })
            //上拉加载更多
            this.scroller.on('pullingUp',()=>{
                this.$emit('pullingUp')
            })
 
        },
        methods:{
            scrollTo(x, y, time=300) {
                this.scroller.scrollTo(0, 0, time)
            },
            finishPullUp(){
                this.scroller && this.scroller.finishPullUp()
            },
            refresh(){
                this.scroller && this.scroller.refresh()
            },
            getScrollY(){
                return this.scroller ? this.scroller.y : 0
            }

        }
    }
</script>

<style scoped>

</style>
