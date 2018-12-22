<template>
    <div class="shopcart">
        
        <div class="shop">
            <div class="shop_1" :class="{'changshop':totalPrice>0}" @click="spec">
                <i class="icon iconfont icon-shopping_cart" width="100%" hight="100%"></i>
            </div>
             
             <div :class="{'shop_num':shop_num>0}">
                 {{shop_num}}
             </div>
        </div>

       

        <div class="price" :class="{'changprice':totalPrice>0}">
           ¥{{totalPrice}}
        </div>

        <div class="pes">
           另需配送费¥{{seller.deliveryPrice}}
        </div>

        <div class="pay" :class="pays">
            
               {{pay}}
            
        </div>

         <div v-show="spec_shop" class="spec_shop">
                <div class="spec_shop_1">
                     <div class="shopcar_detail">
                        <div class="shopcar_detail_1" >
                            <div class="shopcar_detail_car" @click="spec_show">
                                <div class="car_name">购物车</div>
                                <div class="car_del" @click="spec_del">清空</div>
                            </div>
                            <div class="shopcar_detail_2" v-for="(food,index) in selectFoods" :key="index">
                               <div class="foodname">{{food.name}}</div> 
                               <div class="shopcar_detail_price">¥{{food.price}}</div>
                               <shopcontrol class="shopcontrol" :food="food"></shopcontrol>
                            </div>
                            
                        </div>
                </div>
          </div>
        </div>
    </div>
</template>

<script>
import shopcontrol from './shopcontrol'
export default {
    props:{
        selectFoods: Array,
    },
    data(){
    return {
      seller:{},
      spec_shop: false
    }
  },
  created () {
    this.$http.get('/api/seller').then((res) => {
      this.seller = res.body.data
    })
  },
    computed:{
        totalPrice(){
            let total = 0
            this.selectFoods.forEach((food) => {
                total += food.price * food.count
            })
            return total;
        },
        pay(){
            if(this.totalPrice === 0){
                return '¥' + this.seller.minPrice + '起送'
            }else if(this.totalPrice < this.seller.minPrice){
                return '¥' + (this.seller.minPrice - this.totalPrice) + '起送'
            }else{
                return '去结算'
            }
        },
        pays(){
            if(this.totalPrice < this.seller.minPrice){
                return 'nopay'
            }else{
                return 'enpay'
            }
        },
        shop_num(){
            let shop = 0;
            this.selectFoods.forEach((food) => {
                shop += food.count
            })
            if(shop>0){
                return shop;
            }else{
                return '';
            }
            
        }
    },
    methods : {
         spec () {
             this.spec_shop = true
        },
        spec_show(){
            this.spec_shop = false
        },
        spec_del(){
            this.selectFoods.forEach((food) =>{
                food.count =0;
            })
        }
    },
    components : {
        shopcontrol
    }
}
</script>

<style>
.shopcart{
    width: 100%;
    height: 99px;
    background-color: #141d27;
    position: relative;
    z-index: 999;
}

.shop{
    float: left;
    color: rgba(255, 255, 255, 0.4);
    width: 124px;
    height: 124px;
    border-radius: 50%;
    top: -30px;
    position: relative;
    background-color: #141d27;  
    display: flex;
    justify-content: center;
    margin-left: 36px;
   z-index: 999
}
.shop i{
    font-size: 48px;
    margin-top: 25px;
    margin-left: 25px;
}

.shop_1{
    border-radius: 50%;
    width: 100px;
    height: 100px;
    background-color: rgba(255, 255, 255, 0.1);
    margin-top: 10px;
    position: relative;
}
.changshop{
    background-color: rgb(0, 160, 220);
    margin-left: -0px;
}
.changshop i{
    color: white;
}
.price{
    color: rgba(255, 255, 255, 0.4);
    float: left;
    font-size: 42px;
    font-weight: 700;
    line-height: 48px;
    border-right: 2px solid rgba(255, 255, 255, 0.1);
    width: 74px;
    display: flex;
    margin-top: 24px;
    
}
.changprice{
    color: white;
}
.pes{
    font-size: 22px;
    color: rgba(255, 255, 255, 0.4);
    font-weight: 700;
    line-height: 48px;
    float: left;
    margin-left: 24px;
    margin-top: 24px;
}
.pay{
    width: 210px;
    height: 99px;
    background-color: rgba(255, 255, 255, 0.1);
    float: right;
    font-size: 34px;
    color:  rgba(255, 255, 255, 0.4);
    font-weight: 700;
    line-height: 48px;
    display: flex;
    justify-content: center;
    align-items: center;
}
.nopay{
    color: rgba(255, 255, 255, 0.4); 
    background: #2b333b;
}
.enpay{
     background: #00b43c;
        color: #fff;
}
.shop_num{
    color:rgb(255, 255, 255); 
    font-weight: 700;
    line-height: 32px;
    font-size: 18px;
    background-color: rgb(240, 20, 20);
    box-shadow: 0px 4px 8px 0px rgba(0, 0, 0, 0.4);
    width: 48px;
    height: 38px;
    border-radius: 28px;
    display: flex;
    align-items: center;
    justify-content: center;
    position: absolute;
    margin-left: 30px;
}
.shopcar_detail{
    width: 100%;
    height: 91.5%;
    color: white;
    top: 0px;
    z-index: 99
}
.shopcar_detail .shopcar_detail_1{
    width: 100%;
    position: absolute;
    bottom: 90px;

}
.shopcar_detail_car{
    width: 100%;
    height: 80px;
    background-color: rgba(255, 255, 255,1);
}
.car_name{
    font-size: 28px;
    font-weight: 200px;
    color: rgb(7, 17, 27);
    line-height: 80px;
    padding-left: 36px;
    float: left;
}
.car_del{
    color: rgb(0, 160, 200);
    font-size: 24px;
    line-height: 80px;
    margin-left: 550px;
}
.shopcar_detail .shopcar_detail_1 .shopcar_detail_2{
    height: 100px;
    background-color: rgb(255, 255, 255);
    color: rgb(7, 17, 27);
    border-bottom: 1px solid rgba(7, 17, 27, 0.1);
    display: flex;
    align-items: center;
    font-size: 28px;
    line-height: 48px;
    padding-left: 36px;
    display: flex;
    justify-content: space-between;
}
.shopcar_detail_price{
    color: red;
    font-weight: bold;
    right: 0;
}
.shopcontrol{
   color:rgb(0, 160, 200);
   width:200px;
   height: 100px;
   display: flex;
   justify-content: flex-end;
   
}
.foodname{
    width: 400px;
    
}
</style>
