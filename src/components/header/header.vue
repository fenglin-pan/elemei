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
            <div v-if='seller.supports' class='support-border' @click="showDetail()">
                <span class='count'>{{seller.supports.length}}个</span>
                <i class='icon-keyboard_arrow_right'></i>
            </div>
        </div>
        <div class='bulletin' @click="showDetail()">
            <span class='bulletin-title'></span>
            <span class='bulletin-text'>{{seller.bulletin}}</span>
            <i class='icon-keyboard_arrow_right'></i>
        </div>
        <div class='bg' @click="showDetail()">
            <img width='100%' height='100%' :src="seller.avatar" >
        </div>
        <div v-show='detailShow' class='detail'>
            <div class='detail-wrapper clearfix'>
                <div class='detail-main'>
                    <h1 class='name'>{{seller.name}}</h1>
                    <div class="star-wrapper">
                        <star :size='48' :score='seller.score' ></star>
                    </div>
                    <div class="title">
                        <div class='line'></div>
                        <div class='text'>优惠信息</div>
                        <div class='line'></div>
                    </div>
                    <ul v-if='seller.supports' class="supports">
                        <li v-for="(item,index) in seller.supports" class='support-item'>
                            <span class='icon' :class='classMap[seller.supports[index].type]'></span>
                            <div class='text'>{{seller.supports[index].description}}</div>
                        </li>
                    </ul>
                    <div class="title">
                        <div class='line'></div>
                        <div class='text'>商家公告</div>
                        <div class='line'></div>
                    </div>
                    <div class='bulletin-detail'>
                        <div class='content'>{{seller.bulletin}}</div>
                    </div>
                </div>
            </div>
            <div class='detail-close' @click="closeShow()">
                <i class='icon-close'></i>
            </div>
        </div>
    </div>
    
</template>

<script>
import star from '../../components/star/star'
export default {
    data(){
        return{
            detailShow:false
        }
    },
    props:{
        seller:{
           type:Object
        }
    },
    // 绑定classMap，对应下标获取相应class名
    created(){
        this.classMap=['decrease','discount','special','invoice','guarantee'];
    },
    components:{
        star
    },
    methods:{
        showDetail(){
            this.detailShow = true;
        },
        closeShow(){
            this.detailShow = false;
        }
    }
}
</script>

<style lang='stylus' rel='stylesheet/stylus'>
@import "../../common/stylus/mixin.styl";
.header
    position :relative 
    color :#ffffff
    overflow :hidden
    background:rgba(7,17,27,0.2)
    .content-wrapper
        position :relative
        font-size :0
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
        .support-border
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
                font-size: 16px
    .bulletin
        position :relative
        height :28px
        line-height :28px
        padding :0 22px 0 12px
        white-space :nowrap
        overflow :hidden
        text-overflow :ellipsis
        background :rgba(7,17,27,0.2)
        .bulletin-title
            display :inline-block
            vertical-align :top
            margin-top :10px
            width :22px
            height :12px
            bg-image :('../../../static/images/bulletin')
            background-size :cover
            background-repeat :no-repeat
        .bulletin-text
            font-size :10px
            margin: 0 4px
        .icon-keyboard_arrow_right
            position :absolute
            font-size :10px
            right :12px
            top : 10px

    .bg
        position :absolute
        left :0
        top :0
        width :100%
        height :100%
        z-index:-1
        filter:blur(10px)
    .detail
        position :fixed
        z-index :100
        top :0
        left :0
        width :100%
        height :100%
        overflow :auto
        background :rgba(7,17,27,0.8)
        .detail-wrapper
            width:100%
            min-height :100%  
            .detail-main
                margin-top :64px
                padding-bottom :64px
                .name
                    font-size :16px
                    line-height :16px
                    text-align :center
                    font-weight :700
                .star-wrapper
                    margin-top :18px
                    padding:2px 0
                    text-align:center
                .title
                    display :flex
                    width :80%
                    margin :28px auto 24px auto
                    .line
                        flex: 1
                        position  :relative
                        top:-6px
                        border-bottom :1px solid rgba(255,255,255,0.2)
                    .text
                        padding : 0 12px
                        font-size :14px
                        font-weight :700
                .supports
                    width:80%
                    margin :0 auto
                    .support-item    
                        padding :0 12px
                        margin-bottom : 12px
                        font-size :0
                        &:last-child
                            margin-bottom :0
                        .icon
                            display :inline-block
                            float :left
                            width :16px
                            height 16px
                            vertical-align :top
                            margin-right :6px
                            background-size :cover
                            background-repeat :no-repeat
                            &.decrease
                                bg-image("../../../static/images/decrease_2")    
                            &.discount
                                bg-image("../../../static/images/discount_2")                    
                            &.invoice
                                bg-image("../../../static/images/invoice_2")    
                            &.guarantee
                                bg-image("../../../static/images/guarantee_2")    
                            &.special 
                                bg-image("../../../static/images/special_2")
                        .text
                            font-size :12px
                            line-height :16px
                .bulletin-detail
                    width :80%  
                    margin :0 auto 
                    .content
                        padding:0 12px
                        line-height : 24px
                        font-size :12px
        .detail-close
            position :relative
            width :32px
            height :32px
            margin:-64px auto;
            font-size :32px
            clear :both
</style>

