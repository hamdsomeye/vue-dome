<template>
  <div class="ratings">
    <div class="ratings_left">
          <div class="ratings_score">{{seller.score}}</div>
          <div class="zonghe">综合评分</div>
          <div class="rankRate">高于周边商家{{seller.rankRate}}%</div>
    </div>

    <div style="padding-top:10px;">
          <div class="service">
          <div class="service_attitude">服务态度</div>
          <star :size="48" :score="seller.serviceScore" class="servicescore"></star>
          <div class="service_num">{{seller.serviceScore}}</div>
    </div>

    <div class="service">
        <div class="service_attitude">食物态度</div>
        <star :size="48" :score="seller.foodScore" class="servicescore"></star>
        <div class="service_num">{{seller.foodScore}}</div>
    </div>

    <div class="delivery_2">
        <div class="delivery_1">送达时间</div>
        <div class="delivery_time">{{seller.minPrice}}分钟</div>
      </div>

    </div>

     <div class="line"></div>

     <div class="praise">
            <div @click="select(2)" class="praise_1">全部</div>
            <div  @click="select(0)" class="praise_1">好评</div>
            <div @click="select(1)" class="praise_1">差评</div>
      </div>

      <div class="line_1"></div>
      <div class="select">
      <div class="select_i"><i :class="{'on':onlyContent}" id="icon" class="icon iconfont icon-check_circle" @click="ratings123()"></i></div>
       <div class="select_name">只看有内容的评价</div>
       </div>
       <div class="rating_people" ref="peopleWrapper">
         <ul>
          <li v-show="needShow(item.rateType , item.text)" v-for="(item,index) in ratings" :key="index" class="food-lists">
            <div class="people">
                <div class="rating_img"><img :src="item.avatar" alt="" width="100%"></div>
                <div class="rating_name">{{item.username}}</div>
                <div class="rating_star"><star :size="48" :score="item.score"></star></div>
                <div class="rating_text" id="rating_text">{{item.text}}</div>
            </div>
          
        </li>
        </ul>
       </div>
      </div>

      
      
 
</template>

<script>
import star from '@/components/star/star'
import shopcart from '@/components/shopcart/shopcart'
import BScroll from 'better-scroll';
export default {
  data(){
    return {
      seller:{},
      ratings:[],
      listHeight: [],
      scrollY: 0,
      selectType: 2,
      onlyContent: false,
    }
  },
  created () {
    this.$http.get('/api/seller').then((res) => {
      this.seller = res.body.data
    })
    this.$http.get('/api/ratings').then((res) => {
      this.ratings = res.body.data
      console.log(this.ratings)
      // 确保 DOM 已经渲染
        this.$nextTick(() => {
          //better-scroll实现滚动
          this.initScroll();
          // 计算高度
          this.calculateHeight();
        })
    })
  },
  methods:{
    ratings123(){
      let qk = [];
      let icon =  document.getElementById("icon");
      icon.style.color = 'rgb(255, 153, 0)';
        this.onlyContent = !this.onlyContent;
    },
   initScroll() {
      this.peopleScroll = new BScroll(this.$refs.peopleWrapper, {
        // 允许点击
        click:true,
        probeType: 3
      });
      
      //记录当前位置滑动触发
      this.peopleScroll.on('scroll' , (pos) => {
        this.scrollY = Math.abs(Math.round(pos.y));
      })
    },
     calculateHeight() {
      let foodLists = this.$refs.peopleWrapper.getElementsByClassName('food-lists');
      let height = 0;
      this.listHeight.push(height);
      for (let i = 0; i < foodLists.length; i++) {
        let item = foodLists[i];
        height += item.clientHeight;
        this.listHeight.push(height);
      }
    },
    select(type){
        this.selectType = type;
    },
       needShow(type , text) {
       
            if(this.onlyContent && !text) {
                return false
            }
            if(this.selectType === 2){
                return true
                 
            }else{
                return type === this.selectType;
            }
        },
       ratings123(){
        this.onlyContent = !this.onlyContent
    },
  },
  components:{
    star,
    shopcart
  }
}
</script>

<style>
.ratings_left{
  width: 275px;
  height: 175px;
  border-right: 1px solid rgb(147, 153, 159);
  float: left;
}
.ratings_score{
  font-size: 48px;
  color: rgb(255, 153, 0);
  line-height: 56px;
  display: flex;
  justify-content: center;
  margin-top: 36px;
}
.zonghe{
  font-size: 24px;
  color:rgb(7, 17, 27);
  line-height: 24px;
  display: flex;
  justify-content: center;
  margin-top:12px;
}
.rankRate{
  font-size: 20px;
  color: rgb(147, 153, 159);
  line-height: 20px;
  display: flex;
  justify-content: center;
   margin-top:16px;
}
.service{
  width: 364px;
  height: 50px;
  margin-left: 275px;
  display: flex;
  align-items: center;
}
.service_attitude{
  font-size: 24px;
  color:rgb(7, 17, 27);
  line-height: 36px;
  float: left;
  margin-left: 14px;
}
.servicescore{
  float: left;
  margin-left: 14px;
}
.service_num{
  font-size: 24px;
  color:rgb(255, 153, 0);
  line-height: 36px;
  margin-left: 14px;
}
.delivery_2{
  width: 364px;
  height: 50px;
  margin-left: 275px;
  display: flex;
  align-items: center
}
.delivery_1{
  font-size: 24px;
  color:rgb(7, 17, 27);
  line-height: 36px;
  margin-left: 14px;
}
.delivery_time{
  font-size: 24px;
  color:rgb(147, 153, 159);
  line-height: 36px;
  margin-left: 14px;
}
.line{
  width: 100%;
  height: 36px;
  background-color: rgb(147, 153, 159,0.2);
  margin-top: 36px;
}
.praise{
  width: 100%;
  height: 120px;
}
.praise_1{
    width: 100px;
    height: 60px;
    background-color: rgb(0, 160, 200);
    float: left;
    margin-left: 20px;
    font-size: 24px;
    color: rgb(255, 255, 255);
    display: flex;
    align-items: center;
    justify-content: center;
    margin-top: 36px;
}
.select .on{
  color:rgb(0, 160, 200);
}
.line_1 {
   width: 100%;
  height: 8px;
  border-bottom: 3px solid rgb(147, 153, 159,0.2);
}
.select{
  width: 100%;
  height: 80px;
  border-bottom: 3px solid rgb(147, 153, 159,0.2);
}
.select_i{
  height: 65px;
  float: left;
  display: flex;
  align-items: center;
  margin-left: 20px;
}
.select i{
  font-size: 48px;
  color: rgb(147, 153, 159,0.7);
}
.select_name{
  height: 85px;
  display: flex;
  align-items: center;
  font-size: 24px;
  padding-left: 20px;
  color: rgb(147, 153, 159);
}
.rating_people{
  width: 100%;
  height: 300px;
  margin-top: 36px;
  overflow: hidden;
   position: relative;
}
.people{
  width: 100%;
  height: 150px;;
}
.rating_img{
  width: 64px;
  height: 120px;
  margin-left: 36px;
  float: left;
}
.rating_name{
  padding-left: 124px;
}
.rating_star{
   padding-left: 124px;
    margin-top: 8px;
}
.rating_text{
  font-size: 24px;
  line-height: 36px;
  color:rgb(7, 17, 27);
  padding-left: 124px;
  margin-top: 12px;
}
</style>