<template>
  <div id="home">

    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
    <tab-control
      :titles="['流行', '新款', '精选']"
      @tabClick="tabClick"
      class="tab-control"
      ref="tabControl1"
      v-show="isFixed" ></tab-control>

    <scroll class="content"
            ref="scroll"
            :probe-type="3"
            @getPosition="getPosition"
            :is-load="isLoad"
            @pullingUp="pullingUp"
    >
      <home-swiper :banners="banners" @isLoadFinish="isLoadFinish"></home-swiper>
      <recommend-view :recommends="recommends"></recommend-view>
      <feature-view/>
      <tab-control
        :titles="['流行', '新款', '精选']"
        @tabClick="tabClick" ref="tabControl"></tab-control>
      <goods-list :goods="showIndex"/>
    </scroll>

    <back-top @click.native="backClick" v-show="this.isBackShow"></back-top>

  </div>
</template>

<script>

import NavBar from "../../components/common/navbar/NavBar";
import Scroll from "../../components/common/scroll/Scroll";

import TabControl from "../../components/content/tabControl/TabControl";
import GoodsList from "../../components/content/goods/GoodsList";
import BackTop from "../../components/content/backtop/BackTop";


import HomeSwiper from "./homeComps/HomeSwiper";
import RecommendView from "./homeComps/RecommendView";
import FeatureView from "./homeComps/FeatureView";

import {getHomeMultidata,getHomeGoods} from "../../network/home";

import {debounce} from "../../common/utils";

export default {
  name: "Home",
  components: {
      NavBar,
      Scroll,
      TabControl,
      GoodsList,
      BackTop,
      HomeSwiper,
      RecommendView,
      FeatureView,



    },
  data(){
      return{
        banners: [],
        recommends: [],
        goods:{
          'pop': {page:0,list: []},
          'new': {page:0,list: []},
          'sell': {page:0,list: []}
        },
        currentType: 'pop',
        isBackShow: false,
        isLoad: true,
        tabOffSet: 0,
        isFixed: false,
        saveY: 0

      }
    },

  created() {
    //不要再这里写refs 很有可能引用不到
     this.getHomeMultidata();

      this.getHomeGoods('pop');
      this.getHomeGoods('new');
      this.getHomeGoods('sell');
    },
  mounted() {

    const refrence = debounce(this.$refs.scroll.refresh,200)
    this.$bus.$on('itemImageLoad',() =>{
      // console.log('-----');
      // this.$refs.scroll.refresh()
      // this.$refs.scroll.bScroll.refresh()
      refrence()
    });
  },
  destroyed() {
    console.log('111');
  },
  activated() {
    this.$refs.scroll.refresh()
    this.$refs.scroll.scrollTo(0,this.saveY,1)


  },
  deactivated() {
    this.saveY = this.$refs.scroll.getY

  },
  computed:{
      showIndex(){
        return this.goods[this.currentType].list
      }

  },
  methods:{


    tabClick(index){
        switch (index) {
          case 0:
            this.currentType ='pop'
                break
          case 1:
            this.currentType='new'
                break
          case 2:
            this.currentType='sell'
                break

        }
        this.$refs.tabControl1.currentIndex=index
        this.$refs.tabControl.currentIndex=index
      },
    backClick(){
      this.$refs.scroll.scrollTo(0,0,300)
    },
    getPosition(position){
      this.isFixed= (-position.y)>this.tabOffSet
      this.isBackShow = -position.y > 1000
    },
    pullingUp(){
      this.getHomeGoods(this.currentType);


    },
    isLoadFinish(){
     this.tabOffSet = this.$refs.tabControl.$el.offsetTop


    },


    getHomeMultidata(){
        getHomeMultidata().then(res =>{
          // console.log(res);
          this.banners = res.data.banner.list;
          this.recommends = res.data.recommend.list;
        })
      },
    getHomeGoods(type) {
        const page = this.goods[type].page + 1
        getHomeGoods(type, page).then(res => {
          this.goods[type].list.push(...res.data.list)
          this.goods[type].page += 1
          // console.log(res);

          this.$refs.scroll.finishLoad()
        })
      }
  }

  }

</script>

<style scoped>
  #home {
    /*padding-top: 44px;*/
    height: 100vh;
    position: relative;
  }

  .home-nav {
    background-color: var(--color-tint);
    color: #fff;

    /*position: fixed;*/
    /*left: 0;*/
    /*right: 0;*/
    /*top: 0;*/
    /*z-index: 9;*/
  }

  .tab-control {
    position: relative;
    /*top: 44px;*/
    z-index: 9;
  }

  .content {
    overflow: hidden;

    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
  }

  /*.content {*/
  /*  height: calc(100% - 93px);*/
  /*  overflow: hidden;*/
  /*  margin-top: 44px;*/
  /*}*/
</style>
