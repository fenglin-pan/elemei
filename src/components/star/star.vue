<template>
    <div class='star' :class='starType'>
        <span v-for='item in items' :class="item" class='star-item'> </span>
    </div>
</template>

<script>
    const LENGTH = 5 ;
    const CLS_on = 'on';
    const CLS_HALF = 'half';
    const CLS_OFF = 'off';
export default {
    props:{
        size:{
            type:Number
        },
        score:{
            type:Number
        }
    },
    computed:{
        starType(){
            return 'star-'+this.size;
        },
        items(){
            let result = [];
            let score = Math.floor(this.score*2) / 2;
            let hasDecimal = score %1 !== 0;//检查是否是小数
            let integer = Math.floor(score);
            // 循环整数，往result内放全星
            for(let i = 0;i < integer;i++){
                result.push(CLS_on);
            }
            //如果有小数，则有半星，往result内加半星
            if(hasDecimal){
                result.push(CLS_HALF);
            }
            // 如果长度小于5，往数组内加入暗星
            while(result.length < 5){
                result.push(CLS_OFF);
            }
            return result;
        }
    }
    
}
</script>

<style lang='stylus' rel='stylesheet/stylus'>
@import "../../common/stylus/mixin.styl";
.star
    font-size :0
    .star-item
        display :inline-block
    &.star-48
        .star-item
            width :20px
            height :20px
            margin-right :22px
            background-size :cover
            &:last-child
                margin :0
            &.on
                bg-image("../../../static/images/star48_on")
            &.half
                bg-image("../../../static/images/star48_half")
            &.off
                bg-image("../../../static/images/star48_off")
    &.star-36
        .star-item
            width :15px
            height :15px
            margin-right :6px
            background-size :cover
            &:last-child
                margin :0
            &.on
                bg-image("../../../static/images/star36_on")
            &.half
                bg-image("../../../static/images/star36_half")
            &.off
                bg-image("../../../static/images/star36_off")
    &.star-24
        .star-item
            width :10px
            height :10px
            margin-right :3px
            background-size :cover
            &:last-child
                margin :0
            &.on
                bg-image("../../../static/images/star24_on")
            &.half
                bg-image("../../../static/images/star24_half")
            &.off
                bg-image("../../../static/images/star24_off")
</style>
