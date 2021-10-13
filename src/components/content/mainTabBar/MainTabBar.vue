<template>
  <tab-bar>
     <!-- path="/home" 点击哪个事件就改变路径 -->
     <tab-bar-item path="/home">
       <img slot="item-icon" src="../../../assets/img/tabbar/shouye.svg" alt="">
       <img slot="item-icon-actived" src="../../../assets/img/tabbar/redshouye.svg" alt="">
       <div slot="item-text">首页</div>
     </tab-bar-item>
     <tab-bar-item path="/category">
       <img slot="item-icon" src="../../../assets/img/tabbar/fenlei.svg" alt="">
       <img slot="item-icon-actived" src="../../../assets/img/tabbar/redfenlei.svg" alt="">
       <div slot="item-text">分类</div>
     </tab-bar-item>
     <tab-bar-item path="/cart">
       <img slot="item-icon" src="../../../assets/img/tabbar/gouwuchekong.svg" alt="">
       <img slot="item-icon-actived" src="../../../assets/img/tabbar/redgouwuchekong.svg" alt="">
       <div slot="item-text">购物车</div>
     </tab-bar-item>
     <tab-bar-item path="/profile">
       <img slot="item-icon" src="../../../assets/img/tabbar/wode.svg" alt="">
       <img slot="item-icon-actived" src="../../../assets/img/tabbar/redwode.svg" alt="">
       <div slot="item-text">我的</div>
     </tab-bar-item>
   </tab-bar>
</template>

<script>
import TabBar from '../../common/tabbar/TabBar'
import TabBarItem from '../../common/tabbar/TabBarItem'
export default {
    name: "MainTabBar",
    components: {
        TabBar,
        TabBarItem
    }
}
</script>

<style>

</style>