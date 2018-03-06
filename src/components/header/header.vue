<template> 
    <div class = 'header'>
        <div class='content-wrapper'>
            <div class='avatar'>
                <img width='64' height='64' :src="seller.avatar" >
            </div>
            <div class='content'>
                <div class='title'>
                    <span class='brand'></span>
                    <span class='name'>{{seller.name}}</span>
                </div>
                <div class='description'>
                    {{seller.description}}/{{seller.deliveryTime}}分钟送达
                </div>
                <!-- v-if='seller.supports'的作用：请求数据是个异步过程 -->
                <div v-if='seller.supports' class="support">
                    <span class='icon' :class='classMap[seller.supports[0].type]'></span>
                    <span class='text' >{{seller.supports[0].description}}</span>
                </div>
            </div>
            <div v-if='seller.supports' classs='support-count' >
                <span class='count'>{{seller.supports.length}}个</span>
                <i class='iconfont icon-keyboard_arrow_right'></i>
            </div>
        </div>
        <div class='bulletin'></div>
    
    </div>
    
</template>

<script>
export default {
    props:{
        seller:{
           type:Object
        }
    },
    // 绑定classMap，对应下标获取相应class名
    created(){
        this.classMap=['decrease','discount','special','invoice','guarantee']
    }
}
</script>

<style lang='stylus' rel='stylesheet/stylus'>
@import "../../common/stylus/mixin.styl";
.header
    color :#ffffff
    background :#999
    font-size :0;
    .content-wrapper
        position: relative
        padding :24px 12px 18px 18px
        .avatar
            display :inline-block
            vertical-align :top
            img
                border-radius :2px
        .content
            display :inline-block
            font-size :14px
            margin-left :16px
            .title
                margin :2px 0 8px 0
                .brand
                    display :inline-block
                    vertical-align :top
                    width :30px
                    height :18px
                    bg-image :('../../../static/images/brand')
                    background-size :cover
                    background-repeat : no-repeat 
                .name
                    margin-left :6px
                    font-size :16px
                    line-height :18px
                    font-weight :bold
            .description
                margin-bottom :10px
                line-height :12px
                font-size :12px 
            .support
                .icon
                    display :inline-block
                    width :12px
                    height :12px
                    margin-right :6px
                    background-size :cover
                    background-repeat:no-repeat
                    &.decrease
                        bg-image("../../../static/images/decrease_1")    
                    &.discount
                        bg-image("../../../static/images/discount_1")                    
                    &.invoice
                        bg-image("../../../static/images/invoice_1")    
                    &.guarantee
                        bg-image("../../../static/images/guarantee_1")    
                    &.special 
                        bg-image("../../../static/images/special_1")  
                .text
                    font-size :10px
                    line-height :10px  
        .support-count
            position: absolute
            right: 12px
            bottom: 14px
            padding: 0 8px
            height: 24px
            line-height: 24px
            border-radius: 14px
            background: rgba(0, 0, 0, 0.2)
            text-align: center
            .count
                vertical-align: top
                font-size: 10px
            .icon-keyboard_arrow_right
                margin-left: 2px
                line-height: 24px
                font-size: 10px

</style>

