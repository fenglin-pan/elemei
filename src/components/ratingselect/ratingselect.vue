<template>
    <div class='ratingselect'>
        <div class='rating-type border-1px'>
            <span class='block positive':class="{'active':selectType===2}" @click='select(2,$event)'>{{desc.all}}<span>{{ratings.length}}</span></span>
            <span class='block positive':class="{'active':selectType===0}" @click='select(0,$event)'>{{desc.positive}}<span>{{positive.length}}</span></span>
            <span class='block negative':class="{'active':selectType===1}" @click='select(1,$event)'>{{desc.negative}}<span>{{negative.lenght}}</span> </span>
        </div>
        <div class="switch" :class="{'on':onlyContent}" @click='toggle($event)'>
            <span class='icon-check_circle'></span>
            <span class="title">只看内容的评价</span>
        </div>
    </div>
</template>

<script>
const POSITIVE = 0;
const NEGATIVE = 1;
const ALL = 2;
export default {
    props:{
        ratings:{
            type:Array,
            default(){
                return []
            }
        },
        selectType:{
            type:Number,
            default:ALL
        },
        onlyContent:{
            type:Boolean,
            default:false
        },
        desc:{
            type:Object,
            default(){
                return{
                    all:'全部',
                    positive:'满意',
                    negative:'不满意'
                }
            }
        }
    },
    computed:{
        positive:function(){
            return this.ratings.filter((ratings)=> ratings.rateType===POSITIVE)
        },
        negative:function(){
            return this.ratings.filter((ratings)=> ratings.rateType===NEGATIVE)
        }
    },
    methods:{
        select(type,event){
            if(!event._constructed){
                return;
            }
            this.$emit("select",type);//子组件向父组件传递参数

        },
        toggle(event){
            if(!event._constructed){
                return;
            }
            this.$emit("toggle")
        }
    }
}
</script>

<style lang='stylus'>
@import "../../common/stylus/mixin.styl";
.rating-type 
    padding :18px 0
    margin :0 18px
    border-1px(rgba(7,17,27,0.1))
    font-size :0
    .block
        display :inline-block
        padding :8px 12px
        border-radius :2px
        margin-right :8px
        color :rgb(77,85,93)
        font-size : 12px
        line-height :16px
        &.active
            color :#fff
        .count
            font-size :8px
            margin-left :2px
        &.positive
            background :rgba(0,160,220,0.2)
            &.active
                background :rgb(0,160,220)
        &.negative
            background :rgba(77,85,93,0.2)
            &.active
                background :rgb(77,85,93)
.switch
    padding:12px 18px 
    line-height 24px
    border-bottom :1px solid rgba(7,17,27,0.1)
    color :rgb(147,153,159)
    font-size :0
    &.on
        .icon-check_circle
            color :#00c850
    .icon-check_circle
        display :inline-block
        vertical-align :middle
        margin-right :4px
        font-size :20px
    .title
        display :inline-block
        font-size :12px
        vertical-align :middle
                
</style>
