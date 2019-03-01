<template>
  <div class="star" >
      <span v-for="itemClass in itemClasses" :class="itemClass" class="star-item"></span>
  </div>
</template>

<script>
const length=5;
const cls_on="on";
const cls_half="half";
const cls_off="off";
export default {
    props:{
      score:{
        type:Number
      }
    },
    computed:{
      itemClasses(){
        let result=[];
        let score=Math.floor(this.score*2)/2;
        let  hasDecimal=score % 1 !==0;
        let integer=Math.floor(score);
        for (let i=0;i<integer;i++){
          result.push(cls_on);
        }
        if(hasDecimal){
          result.push(cls_half);
        }
        while(result.length<length){
          result.push(cls_off);
        }
        return result;
      }
    }
 
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.star-item{
  display:inline-block;
  width:18px;
  height:18px;
  background-size:100% !important;
  margin:0 5px;
}
.star-item.on{background:url("../../static/img/star36_on@2x.png") no-repeat;}
.star-item.half{background:url("../../static/img/star36_half@2x.png")  no-repeat;}
.star-item.off{background:url("../../static/img/star36_off@2x.png")  no-repeat;}
.star{
  text-align:center;
}
</style>
