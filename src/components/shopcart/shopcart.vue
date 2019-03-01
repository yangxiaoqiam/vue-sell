<template>
   <div class="shopcart">
       <div class="content">
           <div class="content-left">
               <div class="price">￥{{totalPrice}}<div class="num" v-show="totalCount>0">{{totalCount}}</div></div>
               <div class="desc">另需配送费{{deliveryPrice}}元</div>
           </div>
           <div class="content-right" :class="payClass">
            {{payDwsc}}
           </div>
       </div>
   </div>
</template>

<script>

export default {
  
props:{
   selectFoods:{
        type:Array,
        default(){
          return [
            {
              prcie:40,
              count:2
            }
          ]
        }
    },
  deliveryPrice:{
    type:Number,
    defalit:0
  },
  minPrice:{
    type:Number,
    defalit:0
  }

},
computed:{
  totalPrice(){
    let total=0;
    this.selectFoods.forEach((food)=>{
      total+=food.prcie*food.count
    });
    return total;
  },
  totalCount(){
    let count=0;
    this.selectFoods.forEach((food)=>{
      count+=food.count
    })
    return count;
  },
  payDwsc(){
    if(this.totalPrice===0){
      return `￥${this.minPrice}元起送`
    } else if(this.totalPrice<this.minPrice){
      let diff=this.minPrice-this.totalPrice;
        return `还差￥${diff}起送`
    } else{
      return '去结算';
    }
  },
   payClass(){
   if(this.totalPrice<this.minPrice){
        return 'not-enough'
    } else{
      return 'enough';
    }
  },
}
 
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
.shopcart{
  position:fixed;
  left:0;
  right:0;
  bottom:0;
  z-index:50;
  height:50px;
  line-height:50px;
  background:#000;
  .content{
    display:flex;
    .content-left{
      flex:1;
      .price{
        display:inline-block;
        font-size:16px;
        font-weight:bold;
        color:#d6d2d2;
        border-right:1px solid #524d4d;
        padding:0 12px;
        height:20px;
        line-height:20px;
        margin-right:10px;
        position:relative;
        .num{
            position:absolute;
            width:25px;
            height:16px;
            line-height:16px;
            font-size:12px;
            font-weight:bold;
            color:#fff;
            background:red;
            border-radius:10px;
            top: -10px;
            right: -13px;
            text-align:center;
        }
      }
       .desc{
        display:inline-block;
        font-size:12px;
        color:#969292;
      }
    }
    .content-right{
      width:105px;
      flex:0 0 105px;
      float: right;
      text-align: center;
      color:#d6d2d2;
      background: #464646;
      font-size:15px;
      &.not-enough{
         background: #464646;
      }
      &.enough{
         background: green;
         color:#fff;
      }
    }
  }
}
</style>
