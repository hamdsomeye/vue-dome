<template>
  <div class="seller" ref="seller_popel">
    <div class="seller_ref">
              <div class="seller_title">
            <div class="seller_title_i"> 
              <i class="icon iconfont icon-favorite" width="100%" hight="100%"></i>
              <div class="seller_title_i_shou">收藏</div>
            </div>
            <div class="seller_title_left">
              <div class="seller_title_name">{{seller.name}}</div>
              <div class="seller_title_star">
                <star class="span2" :size="48" :score="seller.serviceScore"></star>
                <span class="span1">月售{{seller.sellCount}}单</span>
              </div>
            </div>
        </div>

        <div class="seller_title_2">
            <div class="seller_title_2_left_1">
              <div class="seller_title_2_left_1_name">起送价</div>
              <div class="seller_title_2_left_1_price">{{seller.minPrice}}<span>元</span></div>
            </div>

            <div class="seller_title_2_left_1">
              <div class="seller_title_2_left_1_name">商家配送</div>
              <div class="seller_title_2_left_1_price">{{seller.deliveryPrice}}<span>元</span></div>
            </div>

            <div class="seller_title_2_left_1">
              <div class="seller_title_2_left_1_name">平均配送时间</div>
              <div class="seller_title_2_left_1_price">{{seller.deliveryTime}}<span>分钟</span></div>
            </div>
        </div> 

        <div class="seller_line"></div>
        
        <div class="seller_notice">
            <div class="seller_notice_name">
              公告与活动
            </div>
            <div class="seller_notice_contect">
              {{seller.bulletin}}
            </div>
        </div>

        
          <div class="seller_activity">
                  <ul v-if="seller.supports">
                    <li class="seller_activity_1"  v-for="(item,index) in seller.supports" :key="index">
                      <span class="seller_activity_icon" :class="classMap[seller.supports[index].type]"></span>
                      <span class="seller_activity_name">{{seller.supports[index].description}}</span>
                    </li>
                  </ul>
          </div>
      

        <div class="seller_line"></div>

        <div class="seller_info">
            <div class="seller_info_name">商家实景</div>
            <div class="seller_info_pics" v-for="(item,index) in seller.pics" :key="index">
                <img :src="seller.pics[index]" alt="" width="265" height="205">
            </div>
        </div>

        <div class="seller_line"></div>

          <div class="seller_infos">
            <div class="seller_infos_name">商家实景</div>
            <div class="seller_infos_pics" v-for="(item,index) in seller.infos" :key="index">
                <div class="seller_infos_index">{{seller.infos[index]}}</div>
            </div>
        </div>
    </div>
    

  </div>

    

</template>

<script>
import shopcart from '@/components/shopcart/shopcart'
import star from '@/components/star/star'
import BScroll from 'better-scroll';
export default {
  data(){
    return{
       seller:{},
        listHeight: [],
      scrollY: 0
    }
  },
  created(){
    this.$http.get('/api/seller').then((res) => {
      this.seller = res.body.data
       // 确保 DOM 已经渲染
        this.$nextTick(() => {
          //better-scroll实现滚动
          this.initScroll();
          // 计算高度
          this.calculateHeight();
        })
    }),
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
  },
  methods:{
    initScroll() {
      this.sellerScroll = new BScroll(this.$refs.seller_popel, {
        // 允许点击
        click:true,
        probeType: 3
      });
      
      //记录当前位置滑动触发
      this.sellerScroll.on('scroll' , (pos) => {
        this.scrollY = Math.abs(Math.round(pos.y));
      })
    },
     calculateHeight() {
      let sellerLists = this.$refs.seller_popel.getElementsByClassName('seller_ref');
      let height = 0;
      this.listHeight.push(height);
      for (let i = 0; i < sellerLists.length; i++) {
        let item = sellerLists[i];
        height += item.clientHeight;
        this.listHeight.push(height);
      }
    }
  },
  components:{
    star,
    shopcart
  }
}
</script>

<style>

.seller{
  width: 100%;
  height: 730px;
  overflow: hidden;
}
.seller_title{
  width: 100%;
  height: 110px;
  border-bottom: 2px solid rgba(7, 17, 27,0.1);
}

.seller_title_name{
   font-size: 28px;
  line-height: 28px;
  color:rgb(7, 17, 27);
  margin-top: 36px;
  margin-left: 36px;
}
.seller_title_star{
  margin-top: 16px;
  margin-left: 36px;;
  height: 50px;
}
.seller_title_star .span2{
  float: left;
  height: 35px;
  display: flex;
  align-items: center;
}
.seller_title_star .span1{
   font-size: 20px;
  line-height: 36px;
  color:rgb(77, 85, 93);
  top: -20px;
  margin-left: 25px;
}
.seller_title_i{
 float: right;
}
.seller_title_i i{
   font-size: 48px;
  line-height: 48px;
  color:rgb(240, 20, 20);
   margin-right: 36px;
}
.seller_title_i_shou{
  font-size: 20px;
  line-height: 20px;
  color:rgb(77, 85, 93);
  margin-top: 16px;
}
.seller_title_2{
  width: 100%;
  height: 150px;
  border-bottom: 2px solid rgba(7, 17, 27,0.1);
}
.seller_title_2_left_1{
  width: 211px;
  float: left;
  
}
.seller_title_2_left_1_name{
  font-size: 20px;
  line-height: 20px;
  color:rgb(147, 153, 159);
  display: flex;
  justify-content: center;
  margin-top: 36px;
  border-right: 2px solid rgba(7, 17, 27,0.1);
  
}
.seller_title_2_left_1_price{
  font-size: 48px;
  font-weight: 200;
  line-height: 48px;
  color:rgb(7, 17, 27);
  display: flex;
  justify-content: center;
  margin-top: 8px;
  margin-bottom: 26px;
  border-right: 2px solid rgba(7, 17, 27,0.1);
}
.seller_title_2_left_1_price span{
  font-size: 20px;
  line-height: 48px;
  color:rgb(7, 17, 27);
  margin-top: 10px;
}
.seller_line{
  width: 100%;
  height: 30px;
  background-color: rgba(7, 17, 27,0.1);
}
.seller_notice{
  width: 100%;
  height: 400px;
}
.seller_notice_name{
   font-size: 28px;
  line-height: 28px;
  color:rgb(7, 17, 27);
  margin-top: 36px;
  margin-left: 36px;
}
.seller_notice_contect{
   font-size: 28px;
  font-weight: 200;
  line-height: 48px;
  color:rgb(240, 20, 20);
  margin-top: 16px;
  margin-left: 36px;
  margin-right: 24px;
  padding-bottom: 32px;
  border-bottom: 2px solid rgba(7, 17, 27,0.1);
}
.seller_activity_1{
  font-size: 24px;
   font-weight: 200;
  line-height: 32px;
  color:rgb(7, 17, 27);
  border-bottom: 2px solid rgba(7, 17, 27,0.1);
  display: flex;
  align-items: center;
  margin-top: 32px;
  margin-left: 32px;
  padding-bottom: 32px;
}
.seller_activity_icon{
  display: inline-block;
  width: 42px;
  height: 42px;
  vertical-align: top;
  background-repeat: no-repeat;
}
.seller_activity_name{
  margin-left: 12px;
}
.seller_info{
  margin-left: 36px;
  margin-top: 24px;
  height: 500px;
}
.seller_info_name{
  font-size: 28px;
  line-height: 28px;
  color:rgb(7, 17, 27);
  margin-top: 36px;
  margin-left: 36px;
}
.seller_info_pics{
  float: left;
   
}
.seller_info_pics img{
  margin-left: 12px;
  margin-top: 12px;
}

.seller_infos_name{
    font-size: 28px;
  line-height: 28px;
  color:rgb(7, 17, 27);
  margin-top: 36px;
  margin-left: 36px;
  padding-bottom: 24px;
   border-bottom: 2px solid rgba(7, 17, 27,0.1);
}
.seller_infos_pics{
  margin-top: 32px;
  margin-left: 60px;
  padding-bottom: 32px;
  border-bottom: 2px solid rgba(7, 17, 27,0.1);
}
.seller_infos_index{
    font-size: 24px;
  line-height: 32px;
  font-weight: 200;
  color:rgb(7, 17, 27);
}
</style>