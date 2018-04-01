<template>
    <div class='shopcart'>
        <div class='content'>
            <div class='content-left'>
                <div class='logo-wrapper' >
                    <div class='logo':class="{'currentColor':totalCount>0}">
                        <span class='icon-shopping_cart' :class="{'currentColor':totalCount>0}"></span>
                    </div>
                    <div class='num'>{{totalCount}}</div>
                </div>
                <div class='price' :class="{'hightlight':totalPrice>0}">￥{{totalPrice}}元</div>
                <div class='desc'>另需配送费{{deliveryPrice}}元</div>
            </div>
            <div class='content-right'>
                <div class='pay' :class="">
                    {{payDesc}}
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data(){
        return{
          
            
        }
    },
    props:{
        selectFoods:{
            type:Array,
            default(){
                return [
                    {price:20,
                     count:1   
                    }
                ]
            }
        },
        deliveryPrice:{
            type:Number,
            default:0
        },
        minPrice:{
            type:Number,
            default:20
        }   
    },
    computed:{
        totalPrice(){
            let total = 0;
            this.selectFoods.forEach((food)=>{
                total += food.price * food.count
            })
            return total
        },
        totalCount(){
            let count = 0;
            this.selectFoods.forEach(food=>{
                count += food.count
            })
            return count
        },
        payDesc(){
            if(this.totalPrice === 0){
                return `￥${this.minPrice}元起送`//es6写法 省去+号的繁琐
            }else if(this.totalPrice < this.minPrice){
                let diff = this.minPrice-this.totalPrice;
                return `还差￥${diff}元起送`
            }else{
                return '去结算';
            }
        },
        payClass(){

        }
    }
}
</script>

<style lang='stylus'>
    .shopcart
        width :100%
        height :48px
        position:fixed
        bottom :0
        z-index:50
        .content
            display :flex
            background :#141d27
            color : rgba(255,255,255,0.4)
            .content-left
                flex:1
                .logo-wrapper
                    display :inline-block
                    position :relative
                    top :-10px
                    margin :0 12px
                    padding :6px
                    width :56px
                    height :56px
                    box-sizing :border-box
                    vertical-align :top
                    border-radius :50%
                    background :#141d27
                
                    .logo
                        width :100%
                        height :100%
                        border-radius :50%
                        background :#2b343c
                        text-align :center   
                        &.currentColor
                            background :rgb(0,160,220)                
                        .icon-shopping_cart
                            font-size :24px
                            color:#80858a
                            line-height :48px
                            &.currentColor
                                color:#fff
                    .num
                        position :absolute
                        top :0
                        right :0
                        width :24px
                        height :16px
                        line-height :16px
                        text-align :center
                        border-radius :16px
                        font-weight :400
                        font-size :9px
                        background :rgb(240,20,20)
                        box-shadow :0 4px 8px 0 rgba(0,0,0,0.4)
                .price
                    display :inline-block
                    vertical-align :top
                    margin-top :12px
                    padding-right :12px
                    font-size :16px
                    line-height :24px 
                    font-weight :700
                    box-sizing :border-box
                    border-right :1px solid rgba(255,255,255,0.1)   
                    &.hightlight
                        color :#fff          
                .desc
                    display :inline-block
                    vertical-align :top
                    margin :12px 0 0 12px
                    font-size :16px
                    line-height :24px
            .content-right
                flex:0 0 105px;
                width:105px
                .pay
                    height :48px
                    line-height :48px
                    text-align :center
                    font-size :12px
                    font-weight :700
                    background :#2b333b
                    &.payHighLight
                        background :#00b43c
                        color :#fff
</style>
