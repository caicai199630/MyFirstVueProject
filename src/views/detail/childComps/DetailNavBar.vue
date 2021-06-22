<template>
  <div id="detailItem" >
    <nav-bar>
      <div slot="left" class="back" @click="backClick">
        <img src="../../../assets/img/common/back.svg" alt="">
      </div>
      <div slot="center" class="detailNav">
      <div v-for="(item,index) in title"
           class="detailItem"
           :class="{active: index===currentIndex}" @click="titleClick(index)">{{item}}
      </div>
    </div>
    </nav-bar>
  </div>
</template>

<script>
  import NavBar from "../../../components/common/navbar/NavBar";
  export default {
    name: "DetailNavBar",
    components: {
      NavBar
    },
    data(){
      return{
        title: ['商品','参数','评论','推荐'],
        currentIndex: 0
      }
    },
    methods: {
      titleClick(index){
        this.currentIndex=index
      },
      backClick(){
        this.$router.back()
      }
    }
  }
</script>

<style scoped>
  .detailNav {
    display: flex;
    font-size: 14px;
    flex: 1;
  }
  .detailItem {
    flex: 1;
  }
  .active {
    color: var(--color-high-text)
  }
  .back img{
    margin-top: 13px;
  }
</style>
