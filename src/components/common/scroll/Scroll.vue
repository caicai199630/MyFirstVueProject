<template>
    <div class="wrapper" ref="wrapper">
      <div>
        <slot></slot>
      </div>
    </div>
</template>

<script>
  import  BScroll from 'better-scroll'
    export default {
        name: "Scroll",
      data(){
        return{
          bScroll : null
        }
      },
      props: {
        probeType: {
          type: Number,
          default(){
            return null
          }
        },
        isLoad: {
          type: Boolean,
          default() {
            return true;
          }
        }
      },
      mounted() {
          this.bScroll = new BScroll(this.$refs.wrapper,{
            probeType: this.probeType,
            pullUpLoad: this.isLoad,
            click: true,
            observeDOM:true,
            keepAlive: true
          })
        if (this.probeType=== 2 || this.probeType === 3) {
          this.bScroll.on('scroll', (position) => {
            this.$emit('getPosition', position)
          })
        }

        if(this.isLoad){
          this.bScroll.on('pullingUp',()=>{

            this.$emit('pullingUp')

          })


        }





      },
      methods: {
        scrollTo(x,y,time){
       this.bScroll && this.bScroll.scrollTo(x,y,time)
        },
        finishLoad(){
         this.bScroll && this.bScroll.finishPullUp()
        },
        refresh(){
          this.bScroll && this.bScroll.refresh()
        },
        getY(){
          return this.bScroll ? this.bScroll.y : 0
        }



      }
    }
</script>

<style scoped>

</style>
