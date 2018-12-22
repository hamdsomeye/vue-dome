
<template>
    <div id="app">
       <myheader :seller='seller'></myheader>
        <div class="tab">
            <div class="tab-item">
              <router-link to="/goods">商品</router-link>
             </div>
            <div class="tab-item"><router-link to="/ratings" >评价</router-link></div>
            <div class="tab-item"><router-link to="/seller" >商家</router-link></div>
        </div>
        <router-view></router-view>
    </div>
 
</template>

<script>
import myheader from './components/header/header';
export default {
  data(){
    return {
      seller:{}
    }
  },
  created () {
    this.$http.get('/api/seller').then((res) => {
      this.seller = res.body.data
      console.log(this.seller);
    })
  },
    components : {myheader}
}
</script>

<style>
.app{
  position: absolute;
}
    .tab{
        display: flex;
        
        line-height: 40px;
        margin-top: 56px;
        
    }
    .tab-item{
        flex: 1;
        display: inline-block;
        font-size: 28px;
       
      
    }
    .tab-item a {
      width: 100%;
        height:80px;
      text-decoration: none;
      display: block;
      color: rgb(77,85,93);
       display: flex;
        align-items: center;
        justify-content: center;
    }
    .tab-item .router-link-active{
      color: rgb(240,20,20);
    }
</style>
