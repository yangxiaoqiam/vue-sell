<template>
   <div class="cartControl">
      <transition name="slide-fade"> <div class="cart-decrease" v-show="food.count>0" @click="decreadeCart" ><span>-</span></div> </transition>
      <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
      <div class="cart-add" @click="addCart"><span>+</span></div>
   </div>
</template>

<script>
import Vue from 'vue';
export default {
  
props:{
   food:{
       type:Object
    }
},
methods:{
    addCart(event){
      console.log("222");
      if(!event._constructed){
        return;
      }
      if(!this.food.count){
        Vue.set(this.food,'count',1);
        this.food.count=1;
      } else{
        this.food.count++;
      }
    },
     decreadeCart(event){
      if(!event._constructed){
        return;
      }
      if(this.food.count){
        this.food.count--;
      } 
    }
}
 
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
.cartControl{
  >div{
    display:inline-block;
    padding:5px;
    font-size:14px;
  }
  .cart-decrease{
    transition:all 0.4s linear;

    span{
      display:inline-block;
      color:#0e8ece;
      border:1px solid  #0e8ece;
      width:15px;
      height:15px;
      line-height:15px;
      text-align:center;
      border-radius:100%;

    }
  }
  .cart-count{
    display:inline-block;
    padding:0;
  }
  .cart-add{
    span{
      display:inline-block;
      width:15px;
      height:15px;
      line-height:15px;
      text-align:center;
      border-radius:100%;
      background:#0e8ece;
      color:#fff;
    }
  }
}

.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .3s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active for below version 2.1.8 */ {
  transform: translateX(15px);
  opacity: 0;
}
</style>
