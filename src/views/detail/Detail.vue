<template>
  <div>
    <detail-nav-bar ></detail-nav-bar>
    <detail-swiper :top-images="topImages"></detail-swiper>
  </div>

</template>

<script>

  import DetailNavBar from "./childComps/DetailNavBar";
  import {getDetail} from "../../network/detail";
  import DetailSwiper from "./childComps/DetailSwiper";

  export default {
    name: "Detail",
    data(){
      return{
        iid: null,
        topImages: []
      }
    },
    created() {
      this.iid=this.$route.params.iid,
      getDetail(this.iid).then(res =>{
        console.log(res);
        this.topImages = res.result.itemInfo.topImages



      })
    },
    components: {
      DetailNavBar,
      DetailSwiper,
    },
  }
</script>

<style scoped>

</style>
