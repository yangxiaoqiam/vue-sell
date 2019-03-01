<template>
  <div class="header"> 
    <div class="background">
        <img src="http://static.galileo.xiaojukeji.com/static/tms/seller_avatar_256px.jpg">
    </div>
    <div class="content-wrapper">
      <div class="avatar">
          <img width="64" height="64" :src="seller.avatar">
      </div>
      <div class="content">
          <div class="title">
            <span class="brand">品牌</span>
            <span class="name">{{seller.name}}</span>
          </div>

          <div class="description">
            {{seller.description}}/{{seller.deliveryTime}}分钟送达
          </div>
          <div v-if="seller.supports" class="support">
            <span class="icon" :class="classMap[seller.supports[0].type]"></span>
            <span class="text"> {{seller.supports[0].description}}</span>
          </div>
          <div  v-if="seller.supports" class="support-count" @click="showLayer">
              <span>{{seller.supports.length}}</span>
          </div>
      </div>
    </div>
    <div class="bulletin-wrapper">
       <span class="bulletion-title">公告</span>
       <span class="bulletion-text">{{seller.bulletin}}</span>
    </div>
      <div class="layer" v-show="show">
        <div class="layer-wrap">
          <div class="layer-main">
            <h1 class="name" >{{seller.name}}</h1>
            <star :score="4.5"></star>
            <div class="mid-title">
                <div class="line"></div>
                <div class="text">优惠信息</div>
                <div class="line"></div>
            </div>
            <ul v-if="seller.supports" class="layer-supports">
                <li class="support-item" v-for=" (item, index) in seller.supports">
                <span class="icon" :class='classMap[seller.supports[index].type]'></span>
                <span class='text'>{{seller.supports[index].description}}</span>
                </li>
            </ul>
            <div class="mid-title">
                <div class="line"></div>
                <div class="text">商家公告</div>
                <div class="line"></div>
            </div>
            <div class="layer-bulletion">
                <p class="con">{{seller.bulletin}}</p>
            </div>
        </div>
      </div>

        <div class="layer-close" @click="hiddenLayer">+</div>
      </div>
  </div>

</template>

<script>
 import star from '@/components/star/star.vue';
export default {
  data(){
    return{
       classMap:['decrease','discount','guarantee','invoice','special'],
       show:false
    }
  },
  props:{
    seller:{
      type:Object
    }
  },
   methods: {
    showLayer: function () {
      this.show = true
    },
    hiddenLayer: function () {
      this.show = false;
    }
  },
    components:{
    star:star
  }

}


</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.header{
  color:#ececec; 
  position:relative;
  background: rgba(41, 37, 37,.2);
  overflow:hidden;
}
.content-wrapper{
  padding:24px 12px 18px 24px;
  font-size:0;
  display: flex;
  position:relative;
}
.avatar{
  display:inline-block
}
.content{
  display:inline-block;
  margin-left:16px;
  font-size:14px;
}
.brand{
   display: inline-block;
    background: red;
    color: #fff;
    padding:0px 3px;
    border-radius: 3px;
    font-size:10px;
}
.description{
  font-size:12px;
  padding: 3px 0;
}
.support{
  font-size:12px;
}
.support-count{
    position:absolute;
    right:20px;
    bottom:15px;
    display: inline-block;
    background: rgba(0,0,0,.3);
    height: 18px;
    line-height: 18px;
    padding: 0 10px;
    border-radius: 5px;
}
.bulletin-wrapper{
  font-size:12px;
  background: rgba(0,0,0,.3);
  height:30px;
  line-height:30px;
  padding:0 10px;
  overflow: hidden;
  text-overflow:ellipsis;
  white-space: nowrap;
  position: relative;
}
.bulletin-wrapper .bulletion-title{
  background:#fff;
  color: #5d5c5c;
}
.background{
  position:absolute;
  left:0;
  top:0;
  width:100%;
  height:100%;
  filter:blur(7px)
}
.background img{
  width:100%;
  height:100%;
}
.layer{
  position:fixed;
  left:0;
  top:0;
  width:100%;
  height:100%;
  background:rgba(0,0,0,.7);
  overflow:auto;
}
.layer-wrap{
  min-height:100%;
  padding:0 15px;
}
.layer-wrap .layer-main{
   padding:65px 0;
}
.layer-close{
  width:50px;
  color:#fff;
  margin:-60px auto 0 auto;
  width: 55px;
  margin: -60px auto 0 auto;
  font-size:45px;
  transform:rotate(45deg);
}
.layer-main .name{
  font-size:16px;
  font-weight: bold;
  text-align:center;
}
.mid-title{
  display:flex;
  widht:80%;
  margin:15px auto 15px auto;
}
.mid-title .text{
  padding:0 15px;
  font-size:14px;
  font-weight:bold;
}
.mid-title .line{
  flex:1;
  position:relative;
  top:-6px;
  border-bottom:1px solid rgba(255,255,255,.2)
}
.layer-supports{
  font-size:12px;
  list-style:none;
  margin:0;
  padding:0 10px;
}
.layer-supports li{
  padding:5px 0;
  display:flex;;
}
.layer-supports li .icon{
  display:inline-block;
  width:18px;
  height:18px;
  background-size:100% !important;
  margin-right:10px;
}
.layer-supports li .icon.decrease{
    background:url("../../static/img/decrease_1@2x.png")  no-repeat;
}
.layer-supports li .icon.discount{
    background:url("../../static/img/discount_1@2x.png")  no-repeat;
}
.layer-supports li .icon.guarantee{
    background:url("../../static/img/guarantee_1@2x.png")  no-repeat;
}
.layer-supports li .icon.invoice{
    background:url("../../static/img/invoice_1@2x.png")  no-repeat;
}
.layer-supports li .icon.special{
    background:url("../../static/img/special_1@2x.png")  no-repeat;
}
.layer-bulletion p{
  font-size:12px;
  line-height:2;
  padding:0 10px;
}
</style>
