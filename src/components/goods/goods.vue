<template>
<!-- 父设高，子继承父，over-flow:hidden隐藏 -->
  <div class="goods" >
    <div class="item" ref="menuWrapper">
      <ul>
        <li v-for="(item,index) in goods" :key="index" class="menu-item" :class="{'current':currentIndex === index}" @click="selectMenu(index)">
          <span>{{item.name}}</span>
        </li>
      </ul>
    </div>
    
    <div class="item_right" ref="foodsWrapper">
        <ul>
          <li v-for="(item,index) in goods" :key="index" class="title_right">
              <div class="title_name"><h1>{{item.name}}</h1></div>
              <ul>
                  <li v-for="(food,index) in item.foods" :key="index" class="food-item">
                  <div class="icon"  @click="selectFood(food)">
                     <img width="114" height="114" :src="food.icon" alt="">
                  </div>

                  <div class="icon_title">
                    <span>{{food.name}}</span>
                  </div>
                  <div class="icon_title_1">
                    {{food.description}}
                    <div class="extra">
                      <span class="count">月售{{food.sellCount}}份</span>
                      <span>好评率{{food.rating}}%</span>
                    </div>

                    <div class="money">
                      <span>¥{{food.price}}</span>
                      <!-- <span class="old" v-show="food.oldPrice">¥{{food.oldPrice}}</span> -->
                    </div>

                    <div class="item_right_cartcontrol">
                      <cartcontrol :food = "food"></cartcontrol>
                    </div>
                  </div>
                 
                </li>
              </ul>
          </li>
        </ul>
    </div>
    <shopcart :select-foods="selectFoods" ></shopcart>
    <food :food="selectedFood" ref="mychild"></food>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
import shopcart from '@/components/shopcart/shopcart'
import cartcontrol from '@/components/cartcontrol/cartcontrol'
import food from '@/components/food/food'
export default {
  data(){
    return{
      goods : [],
      listHeight: [],
      scrollY: 0,
       selectedFood: {},
       foods:{}
    }
     
  },
  computed:{
    currentIndex() {
      for (let i = 0; i < this.listHeight.length; i++) {
        let height1 = this.listHeight[i];
        let height2 = this.listHeight[i + 1];
        if(!height2 || this.scrollY >= height1 && this.scrollY < height2) {
          return i;
        }
      }
      return 0;
    },
    selectFoods() {
      let foods = [];
      this.goods.forEach((good) => {
        good.foods.forEach((food) => {
          if(food.count){
            foods.push(food);
          }
        })

      })
      
      this.$emit('changeType',foods)
      return foods;
    }
  },
 created(){
   this.$http.get('/api/goods').then((res) => {

        this.goods = res.body.data
        
      console.log(this.goods)

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
   selectMenu(index) {
      console.log(index)
      let title_right = this.$refs.foodsWrapper.getElementsByClassName('title_right');
      let el = title_right[index];
      this.foodsScroll.scrollToElement(el , 300);
    },
   initScroll() {
      this.meunScroll = new BScroll(this.$refs.menuWrapper, {
        // 允许点击
        click:true
      });

      this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
        // probeType: 1 滚动的时候会派发scroll事件，会截流。2滚动的时候实时派发 - scroll事件，不会截流。 3除了实时派发scroll事件，在swipe的情况下仍然能实时派发scroll事件
        probeType: 3,
        // 允许点击
        click:true
      });
      
      //记录当前位置滑动触发
      this.foodsScroll.on('scroll' , (pos) => {
        this.scrollY = Math.abs(Math.round(pos.y));
      })
    },
      // 计算每个分类的位置
    calculateHeight() {
      let title_right = this.$refs.foodsWrapper.getElementsByClassName('title_right');
      let height = 0;
      this.listHeight.push(height);
      for (let i = 0; i < title_right.length; i++) {
        let item = title_right[i];
        height += item.clientHeight;
        this.listHeight.push(height);
      }
    },
    selectFood(food){
      this.selectedFood = food;
      this.$refs.mychild.showFood();
     
    }
  },
components:{
  shopcart,
  cartcontrol,
  food
  }
}
</script>

<style>

.good{
  width: 100%;
  height: 200px;
  overflow: hidden;
}
.item{
  position: absolute;
  float: left;
  /* width: 100%; */
  height: 690px;
  overflow: hidden;
}
.item .menu-item{
  width: 160px;
  height: 108px;
  font-size: 24px;
  color: #000;
  font-weight: 200;
  line-height: 28px;
  background-color:  rgba(7, 17, 27, 0.1);
  display: flex;
  align-items: center;
}
.item .menu-item span{
  margin-left: 24px;
}
.item_right{
  margin-left: 160px;
  position: relative;
  height: 690px;
  overflow: hidden;
  z-index: 1;
}

.item_right .title_name{
  height: 64px;
  color: rgb(147, 153, 159);
    background-color:  rgba(7, 17, 27, 0.1);
    border-left:6px solid rgba(10, 22, 34, 0.3);
    display: flex;
    align-items: center;
    font-size: 24px;
}
.food-item{
  position: relative;
  width: 100%;
  height: 114px;
  margin-top: 36px;
  margin-bottom: 36px;
}
.icon{
  position: absolute;
}
.icon_title{
  font-size: 28px;
  color:rgb(7, 17, 27);
  line-height: 28px;
  margin-left: 130px;
}
.icon_title_1{
   margin-left: 130px;
   font-size: 20px;
   color: rgb(147, 153, 159);
   line-height: 20px;
   margin-top: 16px;
}
.extra{
  margin-top: 16px;
}
.money{
  font-size: 28px;
  color: red;
  font-weight: bolder;
  font-weight: normal/700;
  line-height: 48px;
  position: absolute;
}
.current{
    background: #fff;
    position: relative;
    margin-top: -1px;
    z-index: 9999;
  }
.item_right_cartcontrol{
    color:rgb(0, 160, 200);
  }
</style>