<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>

    <scroll class="content"  
            ref="scroll" 
            :probe-type="3" 
            @scroll="contentScroll" 
            :pull-up-load="true" 
            @pullingUp="loadMore">
      <home-swiper :banners="banners"></home-swiper>
      <recommend-view :recommends="recommends"></recommend-view> 
      <feature-view></feature-view> 
      <tab-control 
            class="tab-control" 
            :titles="['流行', '新款', '精选']"
            @tabClick="tabClick"
            ></tab-control>
      <goods-list :goods="showGoods"></goods-list>
    </scroll>

    <back-top @click.native="backClick" v-show="isShowBackTop"></back-top>

  </div>
</template>

<script>

import HomeSwiper from './childComps/HomeSwiper'
import RecommendView from './childComps/RecommendView'
import FeatureView from './childComps/FeatureView'

import NavBar from '../../components/common/navbar/NavBar'
import TabControl from '../../components/content/tabControl/TabControl'
import GoodsList from '../../components/content/goods/GoodsList'
import Scroll from '../../components/common/scroll/Scroll'
import BackTop from '../../components/content/backTop/BackTop'


import {getHomeMultidata, getHomeGoods} from "../../network/home"

 
export default {
  name: 'Home',
  components: {
    HomeSwiper,
    RecommendView,
    FeatureView,

    NavBar,
    TabControl,
    GoodsList,
    Scroll,
    BackTop
    
  },
  data(){
    return {
      banners: [],
      recommends: [],
      //首页相关商品 ---> 1.设计数据模型，再去请求数据
      goods: {
        'pop': {page: 0, list: []},
        'new': {page: 0, list: []},
        'sell': {page: 0, list: []},
      },
      currentType: 'pop',
      isShowBackTop: false
    }
  },
  computed: {  
    showGoods(){
      return this.goods[this.currentType].list
    }
  },
  created(){
    //请求多个数据
    this.getHomeMultidata();
  
    //2.首页相关商品 ---> 请求商品数据
    this.getHomeGoods('pop');
    this.getHomeGoods('new');
    this.getHomeGoods('sell');
  },
  methods: {
    /**
     * 事件监听相关方法
     */
    tabClick(index){
      switch(index) {
        case 0:
          this.currentType = 'pop'
          break
        case 1:
          this.currentType = 'new'
          break
        case 2:
          this.currentType = 'sell'
          break
      }
    },
    backClick(){
      //拿到scroll组件对象
      //拿到scroll组件 => 组件内的属性 => 属性再调用方法
      this.$refs.scroll.scroll.scrollTo(0, 0, 500);
    },
    contentScroll(position){
      //console.log(position)
      this.isShowBackTop = (-position.y) > 1000        
    },
    loadMore(){
      console.log('上拉加载更多');  
    },
    /**
     * 网络请求相关的方法
     */
    getHomeMultidata(){
      getHomeMultidata().then(res => {
      //this.result = res;
      this.banners = res.data.banner.list
      this.recommends = res.data.recommend.list
      })
    },
    getHomeGoods(type){

      //动态获取page
      const page = this.goods[type].page + 1

      getHomeGoods(type, page).then(res => {

        //把一个数组放入另一个数组中
        this.goods[type].list.push(...res.data.list)

        //多一组数据，页码+1
        this.goods[type].page += 1
     })
    }
  }
}
</script>

<style scoped>

/* 解决标准流问题 */
#home {
  /* padding-top: 44px; */

  /* vh: 视口 */
  height: 100vh;
  position: relative;
}

.home-nav {
  background-color: var(--color-tint);
  color: #fff;

/* 固定导航栏 */
  position: fixed;
  left: 0;
  right: 0; 
  top: 0;
  z-index: 9;
}

.tab-control {
  position: sticky;
  top: 44px;
  z-index: 8;
}

 .content {
    overflow: hidden;

    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
  }

/* .content {
  height: calc(100% - 98px);
  overflow: hidden;
  margin-top: 44px;
} */
</style>