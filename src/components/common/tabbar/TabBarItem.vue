<template>

  <div class="tab-bar-item" @click="itemClick">

      <div v-if="!isActive"><slot name="item-icon"></slot></div>
      
      <div v-else><slot name="item-icon-actived"></slot></div>  

      <div :style="activeStyle"><slot name="item-text"></slot> </div>
      
  </div>
</template>

<script>
export default {
    name: "TabBarItem",
    props: {
        path: String,
        activeColor: {
            type: String,
            default: 'red'
        }
    },
    data(){
        return {
            //isActive: true
        }
    },
    //需要动态决定的东西，可以放在计算属性里面，再用v-bind绑定
    computed: {
        //动态决定isActive为true还是false
        isActive(){
            // /home -> item1(/home) = true
            // /home -> item1(/category) = false
            return this.$route.path === this.path
        },
        //v-bind绑定style，并把style抽到计算属性，动态实现颜色转换
        activeStyle(){
            return this.isActive ? {color: this.activeColor} : {}
        }
    },
    methods: {
        //监听事件点击，改变路径
        itemClick(){
            this.$router.replace(this.path).catch(err => err)
        }
    }
}
</script>

<style>

.tab-bar-item {
  flex:1;
  text-align: center;
  height: 49px;
  font-size: 14px;
}
.tab-bar-item img{
    width: 24px;
    height: 24px;
    margin-top: 1px;
    vertical-align: middle;
}

</style>