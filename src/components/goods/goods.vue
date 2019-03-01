<template>
  <div class="goods"> 
      <div class="menu-wrapper" ref="menuWrapper">
          <ul>
              <li v-for="(item,index) in goods" class="menu-item"  :class="{'current':currentIndex===index}"  @click="selectMenu(index, $event)">
                 <span class="text">
                    <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
                </span>
              </li>
          </ul>
      </div>
      <div class="foods-wrapper" ref="foodWrapper">
         <ul>
            <li v-for="item in goods" class="food-list  food-list-hook">
               <h3 class="title">{{item.name}}</h3>
               <ul>
                  <!--<li v-for="food in item.foods"class="food-item"  @click="selectFoods(food, $event)" > -->
                  <li v-for="food in item.foods"class="food-item"  >
                      <div class="icon">
                          <img :src="food.icon">
                      </div>
                      <div class="content">
                         <h3 class="name">{{food.name}}</h3>
                         <p class="desc">{{food.description}}</p>
                         <div class="extra">
                             <span class="month-extra">月售{{food.sellCount}}</span>
                             <span>好评率{{food.rating}}%</span>
                         </div>
                         <div class="price">
                            <span class="now">￥{{food.price}}</span>
                            <span class="old">￥{{food.oldPrice}}</span>
                         </div>
                      </div>
                      <div class="cartControl-wrapper">
                          <cartControl :food="food"></cartControl>
                      </div>
                  </li>
               </ul>
            </li>
         </ul>
      </div>
      <shopcart :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
  </div>
</template>

<script>
  import BScroll from 'better-scroll';
  import shopcart  from '@/components/shopcart/shopcart.vue';
  import cartControl from'@/components/cartControl/cartControl.vue';
  const ERR_OK=0;
export default {
  props:{

      seller:{
        type:Object
      }
  },
  data(){
    return{
     goods:[],
     classMap:['decrease','discount','guarantee','invoice','special'],
     listHeight:[],
     scrollY:0
    };
  },                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                
 computed: {
      currentIndex(){ //计算到达哪个区域的区间的时候的对应的索引值
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i]; //当前menu子块的高度
          let height2 = this.listHeight[i + 1]; //下一个menu子块的高度
          //滚动到底部的时候,height2为undefined,需要考虑这种情况
          //需要确定是在两个menu子块的高度区间
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i; //返回这个menu子块的索引
          }
        }
        return 0;
      },
      selectFoods() { //自动将所有的goods.food添加一个count属性,方便做数量运算

        let foods = [];
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              foods.push(food);
            }
          });
        });
        return foods;
      }
    },
  created() {
    this.$http.get('/api/goods').then((response)=>{
        response=response.body;
        if(response.errno===ERR_OK){
          this.goods=response.data;
          this.$nextTick(()=>{
             this.initScroll();
             this.calculateHeight();
          })
          
        }
    })
  },
  methods:{
    selectMenu(index, event){
        if (!event._constructed) { //去掉自带的click事件点击，即pc端直接返回
          return;
        }
        let foodsList = this.$refs.foodWrapper.getElementsByClassName('food-list-hook');
        let el = foodsList[index];
        //类似jump to的功能,通过这个方法,跳转到指定的dom
        this.foodScroll.scrollToElement(el, 300);
      },
    initScroll(){
      this.menuScroll=new BScroll(this.$refs.menuWrapper,{
      click:true
      });
      this.foodScroll=new BScroll(this.$refs.foodWrapper,{
        click:true,
        probeType:3
      });
      this.foodScroll.on('scroll',(pos)=>{
         this.scrollY=Math.abs(Math.round(pos.y));
       })
    },
    
     calculateHeight(){
      let foodList=this.$refs.foodWrapper.getElementsByClassName('food-list-hook');
      let height=0;
      this.listHeight.push(height);
      for (let i=0;i<foodList.length;i++){
        let item = foodList[i];
        height+=item.clientHeight;
        this.listHeight.push(height);
      }
    },
  },
  components:{
    shopcart,
    cartControl

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
  .goods{
    display:flex;
    position:absolute;
    top:175px;
    bottom:45px;
    width:100%;
    overflow:hidden;
    .menu-wrapper{
      flex:0 0 80px;
      width:80px;
      background:#f3f5f7;
      color:#4a4848;
      .menu-item{
          padding:10px 12px;
          font-size:15px;
          border-bottom:1px solid #e2e2e2;
          line-height:15px;
          &.current{
            background:#fff;
          }
          .text{
            display:table-cell;
            vertical-align:middle;
          }
      }
    }
    .foods-wrapper{
      flex:1;
      .food-list{
        position:relative;
        .title{
          background:#f3f5f7;
          padding:10px 5px;
          font-size:15px;
          border-left:2px solid #ced0d2;
          color:#969494;
        }
      }
      .food-item{
        display:flex;
        padding:15px;
         border-bottom: 1px solid #e2e2e2;
         position:relative;
        .content{
          margin-left:10px;
          line-height:1.4;
        }
        .icon{
          img{
            width:55px;
            height:55px;
          }
        }
        .name{
          font-size:15px;
        }
        .desc,.extra,.price{
          font-size:12px;
          color:#969494;
        }
        .month-extra{
          margin-right:5px;
        }
        .price{
          font-weight:bold;
          font-size:14px;
          .now{
            color:red;
            margin-right:5px;
          }
           .old{
             color: #d2d2d2;
             text-decoration: line-through;
          }
        }
      }
    }
  }
  .goods .text .icon{
      display:inline-block;
      width:16px;
      height:16px;
      background-size:100% !important;
      vertical-align: top;
      margin-right:2px;
  }
.goods li .icon.decrease{
    background:url("../../static/img/decrease_3@2x.png")  no-repeat;
}
.goods li .icon.discount{
    background:url("../../static/img/discount_3@2x.png")  no-repeat;
}
.goods li .icon.guarantee{
    background:url("../../static/img/guarantee_3@2x.png")  no-repeat;
}
.goods li .icon.invoice{
    background:url("../../static/img/invoice_3@2x.png")  no-repeat;
}
.goods li .icon.special{
    background:url("../../static/img/special_3@2x.png")  no-repeat;
}

.cartControl-wrapper{
    position: absolute;
    display: inline-block;
    right: 10px;
    bottom: 10px;
  }
</style>
