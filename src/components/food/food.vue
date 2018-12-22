<template>
     <div class="food" v-show="food_show" ref="food" > <!--父 -->
        <div class="food-count" ><!--子，父子实现 better-scroll滚动-->
            <img class="food_img" :src="food.image" alt="">
            <div class="food_title">
                <div class="food_title_name" @click="out">
                    {{food.name}}
                </div>
                <div class="food_title_count">
                <span>月售{{food.sellCount}}份</span> 
                <span>好评率{{food.rating}}%</span>
                </div>
                <div class="food_title_price">
                    ¥{{food.price}}
                </div>
                <div class="food_title_car" @click="foodcar" v-show="food_car">
                    加入购物车
                </div>
                <div class="food_title_cartcontrol" v-show="foodcartcontrol">
                      <cartcontrol :food = "food"></cartcontrol>
                    </div>
            </div>

            <div class="food_info">
                <div class="food_info_title">商品介绍</div>
                <div class="food_info_info">{{food.info}}</div>
            </div>

            <div class="food_select">
                    <div class="food_select_1">商品评价</div>
                     <div class="food_select_praise">
                            <div @click="select(2)" class="food_select_praise_1">全部</div>
                            <div  @click="select(0)" class="food_select_praise_1">好评</div>
                            <div @click="select(1)" class="food_select_praise_1">差评</div>
                    </div>

                    <div class="food_select_line_1"></div>

                    <div class="select">
                        <div class="select_i" ><i :class="{'on':onlyContent}" id="icon" class="icon iconfont icon-check_circle" @click="ratings123()"></i></div>
                        <div class="select_name_1">只看有内容的评价</div>
                    </div>

                      <div class="rating_people">
                        <ul>
                        <li v-show="needShow(item.rateType , item.text)" v-for="(item,index) in food.ratings" :key="index" class="food-lists">
                            <!-- item.rateType=selectType，不等于的都被v-show隐藏掉了 -->
                            <div class="people">
                                <div class="rating_img"><img :src="item.avatar" alt="" width="100%"></div>
                                <div class="rating_name">{{item.username}}</div>
                                <div class="rating_text" id="rating_text">{{item.text}}</div>
                            </div>
                        
                        </li>
                        </ul>
                    </div>
            </div>

            

        </div>
        
    </div>
</template>
<script>
import BScroll from 'better-scroll';
import cartcontrol from '@/components/cartcontrol/cartcontrol'
export default {
    props:{
        food:Object,
         
    },
    data(){
        return{
            food_show : false,
            onlyContent: false,
            selectType: 2,
            foodcartcontrol: false,
            food_car:true
        }
        
    },
    methods:{
     showFood(){
         this.food_show = true;
          this.$nextTick(() => {
                if(!this.scroll){
                    this.scroll = new BScroll(this.$refs.food , {
                        click: true
                    })
                }else{
                    this.scroll.refresh();
                }
            })
     },
     out(){
         this.food_show = false;
     },
      ratings123(){
        this.onlyContent = !this.onlyContent
    },
      select(type){
        this.selectType = type;
    },
    needShow(type , text) {
       
            if(this.onlyContent && !text) {
              //&&两个都为ture
                return false
            }
            if(this.selectType === 2){
                return true
                 
            }else{
                return type === this.selectType;
                //  type === this.selectType 返回true 
            }
        },
    foodcar(){
        this.foodcartcontrol = true;
        this.food_car = false;
    }
 },
 components:{
     cartcontrol
 }
}
</script>
<style>
.food{
    width: 100%;
    position: absolute;
    top:0;
    bottom: 0;
    background-color: #fff;
    z-index: 50;
    height: 1130px;
     overflow: hidden;
}
.food_img{
    height: 750px;
    width: 100%
}
.food_title{
    width: 100%;
    height: 180px;
    border-bottom: 2px solid rgba(7, 17, 27, 0.1);
}
.food_title_name{
    font-size: 28px;
    font-weight: 700;
    color:rgb(7,17,27);
    line-height: 28px;
    margin-top: 36px;
    margin-left: 36px;  
}
.food_title_count{
    font-size: 20px;
    color: rgb(147, 153, 159);
    line-height: 28px;
    margin-top: 16px;
    margin-left: 36px;
}
.food_title_price{
    font-size: 28px;
    color: rgb(240, 20, 20);
    line-height: 28px;
    font-weight: 700;
    padding-top: 36px;
    margin-left: 36px;
    position: absolute;
}
.food_title_car{
    font-size: 20px;
    color:rgb(255, 255, 255);
    line-height: 24px;
    border-radius: 24px;
    background-color: rgb(0,160,220);
    width: 148px;
    height: 48px;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-left: 450px;
    
}
.food_title_cartcontrol{
    margin-left: 250px;
     color:rgb(0, 160, 200);
}
.food_info{
    width: 100%;
    height: 220px;
    margin-top: 32px;
    border-top: 2px solid rgba(7, 17, 27, 0.1);
    border-bottom: 2px solid rgba(7, 17, 27, 0.1);
}
.food_info_title{
    font-size: 28px;
    color:rgb(7,17,27);
    line-height: 28px;
    margin-top: 36px;
    margin-left: 36px;
}
.food_info_info{
    font-size: 24px;
    font-weight: 200;
    color:rgb(77,85,93);
    line-height: 48px;
     margin-top: 12px;
    margin-left: 52px;
}
.food_select{
    margin-top: 32px;
    border-top: 2px solid rgba(7, 17, 27, 0.1);
    width: 100%;
}
.food_select_1{
     font-size: 28px;
    color:rgb(7,17,27);
    line-height: 28px;
    margin-top: 36px;
    margin-left: 36px;
}
.food_select_praise{
    height: 120px;
}
.food_select_praise_1{
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
.food_select_line_1{
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
    width: 100%;
  height: 65px;
  float: left;
  display: flex;
  align-items: center;
  margin-left: 20px;
  position: absolute
}
.select i{
  font-size: 48px;
  color: rgb(147, 153, 159,0.7);
}
.select .on{
  color:rgb(0, 160, 200);
}
.select_name_1{
  height: 85px;
  display: flex;
  align-items: center;
  font-size: 24px;
  padding-left: 70px;
  color: rgb(147, 153, 159);
}
.rating_people{
  width: 100%;
  height: 500px;
  margin-top: 36px;
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