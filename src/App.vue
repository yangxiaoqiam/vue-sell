<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab">
        <div class="tab-item">
           <router-link to="/">商品</router-link>
        </div>
        <div class="tab-item">
            <router-link to="/ratings">评论</router-link>
        </div>
        <div class="tab-item">
          <router-link to="/seller">商家</router-link>
        </div>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
 import header from './components/header/header.vue';
 const ERR_OK=0;
export default {
  name: 'App',
  data(){
    return{
      seller:{}
    }
  },
  created() {
    this.$http.get('/api/seller').then((response)=>{
        response=response.body;
        if(response.errno===ERR_OK){
          this.seller=response.data;
        }
    })
  },
  components:{
    'v-header':header
  }
}
</script>

<style>
body{
  padding:0;
  margin:0;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;

}
.tab{
    display: flex;
    justify-content: center;
    width: 100%;
    align-items: center;
}
.tab .tab-item{
  flex:1;
  text-align:center;
  height:40px;
  line-height:40px;
}
.router-link-exact-active{
  color:#e21a1a
}
.tab{
  border-bottom:1px solid #f1efef;
}

</style>
