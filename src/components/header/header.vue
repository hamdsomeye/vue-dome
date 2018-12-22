<template>
  <div class="header"> 
     
    <div class="header_1">
      <img :src="seller.avatar" width="128" height="128" alt="">
    </div>
    
    <div class="header_4">
       <div class="background">
            <img :src="seller.avatar" width="100%;" height="100%">
        </div>
    </div>

    <div class="header_2">
      <img class="header_2_img" src="./brand@2x.png" alt="">
      <div class="title">{{seller.name}}</div>
      <div class="delivery">{{seller.description}}/{{seller.deliveryTime}}分钟送达</div>
      <div class="reduce" v-if="seller.supports">
        <img src="./decrease_1@2x.png" alt="">
        <div class="reduce_1">{{seller.supports[0].description}}</div>
        
      </div>
      <div class="click" @click="showDetail">
          5个&nbsp;>
      </div>
    
    </div>

    <div class="header_3">
          <img src="./bulletin@2x.png" alt="">
          <div style="padding-left : 8px;">粥品香坊其烹饪粥料的秘方源于中国千年古法,再融和现代制作工艺...</div>
      </div>
    
       
        <div class="detail" v-show="detailShow">
            <div class="detail_title">{{seller.name}}</div>
            <div class="star-wrapper">
                <star :size="48" :score="seller.score"></star>
            </div>
           <div class="title_3">
              <div class="line2"></div>
              <div class="text">优惠信息</div>
              <div class="line1"></div>
            </div>
            <div class="information">
              <ul v-if="seller.supports">
                <li  v-for="(item,index) in seller.supports" :key="index">
                  <span class="icon" :class="classMap[seller.supports[index].type]"></span>
                  <span class="information_text">{{seller.supports[index].description}}</span>
                </li>
              </ul>
            </div>
            <div class="title_3">
              <div class="line2"></div>
              <div class="text">商家公告</div>
              <div class="line1"></div>
            </div>
            <div class="introduce">
              {{seller.bulletin}}
            </div>
             <div class="detail-close" @click="hideDetail">
               <i class="icon iconfont icon-close"></i>
            </div>
        </div>

        
  </div>
</template>

<script>
import star from '@/components/star/star'
export default {
  //接受数据
  props:{
      seller : Object
  },
  data () {
    return {
      detailShow: false
    }
  },
  methods: {
    showDetail () {
      this.detailShow = true
    },
    hideDetail () {
      this.detailShow = false
    }
  },
   created () {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
  },
  components: { star }
}
</script>

<style>

.header{
  width: 100%;
  height: 212px;
  z-index: 99;
}
.header_4{
  width: 100%;
  height: 212px;
  overflow: hidden;
  position: absolute; 
}
.background{
  position: absolute;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
  filter: blur(10px);
}
.header_1{
  margin-top:48px;
  margin-left: 48px;
  position: absolute;
}
.header_2{
  position: absolute;
  margin-top:52px;
  margin-left: 208px;
   float: left;
   width:380px ;
}
.header_2_img{
  float: left;
}
.header_2 .title{
  font-size: 32px;
  color: rgb(255, 255, 255);
  font-weight: bold;
  line-height: 36px;
  padding-left:75px;
}
.header_2 .delivery{
  font-size: 24px;
  color:rgb(255, 255, 255);
  margin-top: 16px;
}
.reduce{
  padding-top: 20px;
  position:absolute;
}
.reduce img{
  position: absolute;
  padding-left: 4px;
}
.reduce .reduce_1{
  width: 100%;
  height: 24px;
  display: flex;
  align-items: center;
  padding-left: 36px;
   color:rgb(255, 255, 255);
}
.click{
  float: right;
  width: 68px;
  height: 48px;
  background-color: rgb(0,0 ,0,0.2);
  border-radius: 16px;
  font-size: 20px;
  color:rgb(255, 255, 255);
  font-weight: 200;
  line-height: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.header_3{
  width: 100%;
  height: 56px;
  background-color: rgb(7, 17, 27,0.2);
  position: absolute;
  margin-top:212px; 
  display: flex;
  align-items: center;
  font-size: 18px;
}
.header_3 img{
  margin-left: 24px;
}
 .decrease{
        background: url('decrease_1@3x.png');
    }
    .discount{
        background: url('decrease_2@3x.png');
    }
    .discount{
        background: url('discount_2@3x.png');
    }
    .special{
        background: url('special_2@3x.png');
    }
    .invoice{
        background: url('invoice_2@3x.png');
    }
    .guarantee{
        background: url('guarantee_2@3x.png');
    }
.detail{
  position: absolute;
  background-color:rgb(0,0 ,0,0.7);
  width: 100%;
  height: 100%;
  z-index: 9999;
}
.detail_title{
  font-size: 32px;
  font-weight: 700;
  color: rgb(255, 255, 255);
  line-height: 32px;
  display: flex;
  justify-content: center;
  padding-top: 128px;

}
.information{
  margin-left: 104px;
  margin-top: 48px;
}
.information_text{
  color: rgb(255, 255, 255);
  font-size: 24px;
  font-weight: 200;
  line-height: 24px;
  margin-left: 25px;
}
.icon{
  display: inline-block;
  width: 32px;
  height: 32px;
  vertical-align: top;
   margin-right: 12px;
  background-size: 16px 16px;
  background-repeat: no-repeat;
}
.detail ul li{
  margin-top: 12px;
}
.title_3{
  display: flex;
  margin-top: 56px;
}
.title_3 .line2{
flex: 1;
position: relative;
top: -6px;
border-bottom: 5px solid rgba(255, 255, 255, 0.2);
margin-left: 54px;
margin-right: 22px;
}
.title_3 .text{
  font-size: 28px;
  font-weight: 700;
  color: rgb(255,255,255);
  line-height: 28px;
}
.title_3 .line1{
  flex: 1;
position: relative;
top: -6px;
border-bottom: 5px solid rgba(255, 255, 255, 0.2);
margin-left: 24px;
margin-right: 72px;
}
.star-wrapper{
 margin-top: 32px;
  padding: 2px 0;
 text-align: center;
  }
.introduce{
  color: rgb(255, 255, 255);
  font-weight: 200;
  font-size: 24px;
  line-height: 48px;
  margin-left: 96px;
  margin-right: 96px;
  margin-top: 24px;
}
 .detail-close{
        position: relative;
        color: aliceblue;
        display: flex;
        justify-content: center;
        margin-top: 60px;
    }
    .detail-close .icon{
        font-size: 64px;
        color: rgb(255, 255, 255,0.5);
    }
</style>