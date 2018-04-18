<template>
<transition name='move'>
    <div v-show='showFlag'class='food' ref='food'>
        <div class='food-content'>
            <div class='image-head'>
                <img :src="food.image">
                <div class='back' @click='back'>
                    <i class='icon-arrow_lift'></i>   
                </div>
            </div>
            <div class='content'>
                <h1 class='title'>{{food.name}}</h1>
                <div class='detail'>
                    <span class='sell-count'>月售{{food.sellCount}}份</span>
                    <span class='rating'>好评率{{food.rating}}</span>
                </div>
                <div class='price'>
                    <span class='now'>￥{{food.price}}</span>
                    <span class='old' v-show='food.oldPrice'>￥{{food.oldPrice}}</span>
                </div>
                <div class='cartcontrol-wrapper'>
                    <cartcontrol :food='food'></cartcontrol>
                </div>
                <div class='buy' v-show="!food.count || food.count ==0" @click.stop.prevent='addFirst($event)'>加入购物车</div>
            </div>
            <split></split>
            <div class='info' v-show = 'food.info'>
                <h1 class='title'>商品信息</h1>
                <div class='text'>{{food.info}}</div>
            </div>
            <split></split>
            <div class='comment'>
                <h1 class='title'>商品评价</h1>
                <ratingselect :ratings='food.ratings':select-type = 'selectType':only-content='onlyContent':desc='desc' @select='select' @toggle='toggle'></ratingselect>
            </div>
        </div>
    </div>
</transition>
</template>

<script>
import BScroll from 'better-scroll'
import Vue from 'vue'
import cartcontrol from '../cartconcontrol/cartconcontrol'
import split from '../split/split'
import ratingselect from '../ratingselect/ratingselect'
const POSITIVE = 0;
const NEGATIVE = 1;
const ALL = 2;
export default {
    props:{
        food:{
            type:Object
        }
    },
    data(){
        return{
            showFlag:false,
            selectType:ALL,
            onlyContent:true,
            desc:{
                all:'全部',
                positive:'满意',
                negative:'不满意'
            }
        }
    },
    methods:{
        //为组件私有的方法，命名方式为_+'name'
        //父组件可调用子组件的方法  反正不可以
        show(){
            this.showFlag = true;
            this.selectType = ALL;
            this.onlyContent = true;
            //$nextTick 是在下次 DOM 更新循环结束之后执行延迟回调，在修改数据之后使用 $nextTick，则可以在回调中获取更新后的 DOM
            this.$nextTick(()=>{
                if(!this.scroll){
                    this.scroll = new BScroll(this.$refs.food,{
                        click:true
                    })
                }else{
                    this.scroll.refresh()
                }
            })
        },
        back(){
            this.showFlag = false;
        },
        addFirst(event){
            if(!event._constructed){
                return
            }
            Vue.set(this.food,'count',1);
        },
        select(type){
            this.selectType = type;
            this.$nextTick(()=>{
                this.scroll.refresh()
            })
        },
        toggle(){
            this.onlyContent = !this.onlyContent;
            this.$nextTick(()=>{
                this.scroll.refresh()
            })
        }
    },
    components:{
        cartcontrol,
        split,
        ratingselect
    }
}
</script>

<style lang='stylus'>
    .food
        position: fixed
        left :0
        top:0
        bottom:48px
        z-index :30
        width :100%
        background :#fff
        transform :translate3d(0,0,0)
        &.move-enter-active,&.move-leave-active
            transition:all 0.2s linear 
        &.move-enter,&.move-leave-active
            transform :translate3d(100%,0,0)
        .image-head
        //图片高度自适应，运用父容器的高度为0，padding-top为100%，img设置绝对定位
            position :relative
            width :100%
            height :0
            padding-top:100%//相对于盒子的宽度计算
            img 
                position :absolute
                top :0
                left :0
                width :100%
                height:100%
            .back
                position :absolute
                top:10px
                left:0
                .icon-arrow_lift
                    display :block
                    padding: 10px//目的增加点击范围
                    font-size :20px
                    color:#fff
        .content
            position :relative
            padding :18px
            .title
                line-height :24px
                margin-bottom :8px
                font-size :14px
                font-weight :700
                color:rgb(7,17,27)
            .detail
                margin-bottom :18px
                line-height :10px
                font-size :0
                height :10px
                .sell-count,.rating
                    font-size :10px
                    color :rgb(147,153,159)
                .sell-count
                    margin-right :18px
            .price
                font-weight :700
                line-height :24px
                .now
                    margin-right :8px
                    font-size :14px
                    color:rgb(240,20,20)
                .old
                    text-decoration :line-through
                    font-size :10px
                    color:rgb(147,153,159)
            .cartcontrol-wrapper
                position :absolute
                right :12px
                bottom:12px
            .buy
                position :absolute
                right :18px
                bottom:18px
                height :24px
                z-index :10
                line-height :24px
                box-sizing :border-box
                padding:0 12px
                font-size :10px
                color:#fff
                border-radius :12px
                background :rgb(0,160,220)
        .info
            padding :18px
            .title
                line-height :14px
                margin-bottom :6px
                font-size :14px
                color:rgb(7,17,27)
            .text
                line-height :24px
                padding :0 8px
                font-size :12px
                color:rgb(77,85,93)
        .comment
            padding-top: 18px
            .title
                line-height :14px
                margin-left 18px
                font-size 14px
                color:rgb(7,17,27)        

            




</style>
