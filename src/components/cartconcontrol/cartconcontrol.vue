<template>
    <div class='cartconcontrol'>
        <transition name='fadeAdd'>
            <div class='decrease ' v-show="food.count>0" @click='decreaseClick'>
                <span class='icon-remove_circle_outline inner'></span>
            </div>
        </transition>
        <div class='count' v-show="food.count>0">
        {{food.count}}
        </div>
        <div class='add icon-add_circle' @click='foodClick'>
        </div>
    </div>
</template>

<script>
import Vue from 'vue'
export default {
    props:{
        food:{
           type:Object,
           return:{
               
           }
        }
    },
    methods:{
        foodClick(event){
            //防止pc端触发多次点击事件
            if(!event._constructed){
                return;
            }
            console.log("click")
            if(!this.food.count){
                Vue.set(this.food,'count',1)
            }else{
                this.food.count ++;
            }
        },
        decreaseClick(event){
            if(!event._constructed){
                return
            }
            if(this.food.count){
                this.food.count --;
            }

        }     
    }
}
</script>

<style lang='stylus'>
.cartconcontrol
    fonst-size :0
    .decrease
        padding :6px
        display :inline-block
        opacity :1
        transition :translate3d(0,0,0)
        .inner
            display :inline-block
            line-height :24px
            font-size :24px
            color :rgb(0,160,220)
            transition :all 0.4s linear 
            transform :rotate(0)
        &.fadeAdd-enter-active,&.fadeAdd-leave-active
            transition :all 0.4s linear 
        &.fadeAdd-enter,&.fadeAdd-leave-active
            opacity :0
            transform :translate3d(24px,0,0)
            .inner
                transform :rotate(180deg)        
    .count
        display :inline-block
        vertical-align :top
        padding-top :6px
        line-height :24px
        text-align :center
        font-size :12px
        color :rgb(147,153,159) 
    .add
        display :inline-block
        padding :6px
        line-height :24px
        font-size :24px
        color :rgb(0,160,220)
</style>
