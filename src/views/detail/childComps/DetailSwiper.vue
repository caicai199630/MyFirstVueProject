<template>
  <swiper class="detail-top-image">
    <swiper-item v-for="item in topImages">
      <img :src="item" alt="">
    </swiper-item>
  </swiper>
</template>

<script>
  import {Swiper,SwiperItem} from 'components/common/swiper/index'
  export default {
    name: "DetailSwiper",
    components: {
      Swiper,
      SwiperItem
    },
    props:{
      topImages:{
        type: Array,
        default(){
          return []
        }
      }

    },
  }
</script>

<style scoped>
  .detail-top-image {
    overflow: hidden;
    height: 300px;
  }

</style>
