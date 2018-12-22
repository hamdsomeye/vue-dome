<template>
<!-- 加号 -->
    
    <div class="cartcontrol">
        
        
          <transition name="move">
            <div class="cartcount_reduce_1" v-show="food.count>0" @click="reducecart">
                <i class="icon iconfont icon-remove_circle_outlin" width="200%" hight="200%"></i>
            </div>
         </transition>
          <div class="cartcount_1" v-show="food.count">{{food.count}}</div>
         <div class="add_1" @click="addCart">
             <i class="icon iconfont icon-add_circle" width="200%" hight="200%"></i>
         </div>
    </div>
</template>

<script>
import Vue from 'vue';
export default {
    props: {
            food: Object
        },
    methods:{
        addCart(){
            if(!this.food.count){
                Vue.set(this.food,'count',1);
            }else{
                this.food.count++;
            }
        },
        reducecart(){
            this.food.count--;
        }
    }
}
</script>

<style>
.cartcontrol{
    height: 50px;;
}
.cartcount_reduce_1{
    font-size: 48px;
    width: 48px;
    height: 48px;
    float: left;
    
}
.cartcount_1{
    padding-left: 24px;
}
.add_1{
    width: 48px;
    height: 48px;
    position: relative;
    margin-left: 24px;
}
.cartcontrol i{
    font-size: 48px;
}


/* 动画 */
    .move-enter-active,.move-leave-active{
        transition:all .4s linear ;
    }
    /* 元素最终状态 */
    .move-enter-active{
        opacity: 1;
        transform: translate3d(0 , 0 , 0) rotate(0);
    }
    /* 元素离开的最终状态 */
    .move-leave-active{
        opacity: 0;
        transform: translate3d(84px , 0 , 0) rotate(360deg);

    }
    /* 元素初始状态 */
    .move-enter,.move-leave{
        opacity: 0;
        transform: translate3d(84px , 0 , 0) rotate(360deg);
    }
</style>


