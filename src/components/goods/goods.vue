<template>
  <div class='goods'>
    <div class='menu-wrapper' ref="menuWrapper">
      <ul>
        <li v-for="(item,index) in goods"  class='menu-item' :class="index == currentIndex?'current':'menu-item'" @click="selectMenu(index,$event)" ref="menuList" >
          <span class='text border-1px' >
            <span v-show='item.type>0'class="icon" :class='classMap[item.type]'></span>{{item.name}}</span>
        </li>
      </ul>
    </div>
    <div class='foods-wrapper'  ref='foodsWrapper'>
      <ul>
        <li v-for='item in goods' ref='foodList'>
          <h1 class='title'>{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class='food-item food-list-hook'>
              <div class='icon' >
                <img :src="food.icon" alt="">
              </div>
              <div class='content'>
                <h2 class='name'>{{food.name}}</h2>
                <div class='desc'>{{food.desciption}}</div>
                <div class='extra'>
                  <span class='count'>月售{{food.sellCount}}份</span>
                  <span >好评率{{food.rating}}</span>
                </div>
                <div class='price'>
                  <span class='now'>￥{{food.price}}</span>
                  <span v-show='food.oldPrice' class='old'>￥{{food.oldPrice}}</span>
                </div>
              </div>
              
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart></shopcart>
  </div>
</template>

<script>
import axios from 'axios'
import shopcart from '../shopcart/shopcart'
import BScroll from 'better-scroll'
export default {
  props:{
    seller:{
       type:Object
    }
  },
  data(){
    return {
      goods:[],
      listHeight:[],
      scrollY:0
    }
  },
  computed:{
    currentIndex() {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i];//获取高度
          let height2 = this.listHeight[i + 1];//下一个高度；即height1、2为区间的上下范围
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {//落在区间或者在最后一个，返回当前的索引
            this._followScroll(i);
            return i;
          }
        }
        return 0;
        console.log(currentIndex)
      }
  },
  created(){
    this.classMap=['decrease','discount','special','invoice','guarantee'];
    axios.get('static/data.json')
      .then(response =>{
        this.goods = response.data.goods;
        this.$nextTick(() => {
          //因为在mouted这个时候，wrapper 的 DOM 已经渲染了，我们可以正确计算它以及它内层 content 的高度，以确保滚动正常。
            this._initScroll();
            this._calculateHeight();
          });
      })
  },
  methods:{
    selectMenu(index, event) {
        if (!event._constructed) {//防止pc端点击出现两次的情况
          return;
        }
        var foodList = this.$refs.foodList;
        // var el = this.listHeight[index];
        // this.foodsScroll.scrollToElement(this.listHeight[index], 300);
        this.foodsScroll.scrollTo(0,-this.listHeight[index],300)
        console.log("实际指向",this.currentIndex)
        console.log('点击的索引',index)
      },
    _initScroll() {
        this.meunScroll = new BScroll(this.$refs.menuWrapper, {
          click: true
        });
        this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
          click: true,
          probeType: 3//实时监测位置
        });
        this.foodsScroll.on('scroll', (pos) => {
          // 判断滑动方向，避免下拉时分类高亮错误（如第一分类商品数量为1时，下拉使得第二分类高亮）
          if (pos.y <= 0) {
            this.scrollY = Math.abs(Math.round(pos.y));
          }
        });
      },
      _calculateHeight() {
        let foodList = this.$refs.foodList;
        let height = 0;
        this.listHeight.push(height);
        for (let i = 0; i < foodList.length; i++) {
          let item = foodList[i];
          height += item.clientHeight;
          this.listHeight.push(height);
        }
      },
      _followScroll(index) {
        let menuList = this.$refs.menuList;
        let el = menuList[index];
        this.meunScroll.scrollToElement(el, 300, 0, -100);
      }
  },
  components:{
    shopcart
  }
  }
</script>

<style lang='stylus'>
@import "../../common/stylus/mixin.styl";

.goods
  display :flex
  position :absolute
  top :174px
  bottom :46px
  width :100%
  overflow:hidden
  .menu-wrapper
    flex:0 0 80px
    width :80px
    background :#f3f5f7
    .menu-item
      display:table
      height :54px
      width :56px
      padding:0 12px
      line-height :14px
      &.current
        position :relative
        z-index:10
        margin-top :-1px
        background :#fff
        font-weight :700
        .text
          border-none()
      .icon
        display :inline-block
        float :left
        width :12px
        height 12px
        vertical-align :top
        margin-right :2px
        background-size :cover
        background-repeat :no-repeat
        &.decrease
          bg-image("../../../static/images/decrease_3")    
        &.discount
          bg-image("../../../static/images/discount_3")                    
        &.invoice
          bg-image("../../../static/images/invoice_3")    
        &.guarantee
          bg-image("../../../static/images/guarantee_3")    
        &.special 
          bg-image("../../../static/images/special_3")    
      .text
        display :table-cell
        width :56px
        vertical-align :middle
        font-size :12px
        border-1px(rgba(7,17,27,0.1))
  .foods-wrapper
    flex:1
    .title
      padding-left :14px
      height :26px
      line-height :26px
      border-left :2px solid #d9dde1
      font-size :12px
      color:rgb(147,153,159)
      background :#f3f5f7
    .food-item
      display:flex
      margin :18px
      padding-bottom:18px
      border-1px(rgba(7,17,27,0.1))
      &:last-child
        border :none
        margin-bottom :0
      .icon
        flex:0 0 57px
        margin-right :10px
      .content
        flex:1
        .name
          margin:2px 0 8px 0
          height :14px
          line-height :14px
          font-size :14px
          color :rgb(7,17,27)
        .desc,.extra
          line-height :10px
          font-size :10px
          color:rgb(147,153,159)
        .desc
            margin-bottom :8px
        .extra
          .count
            margin-right :12px
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
            color:rgb(240,20,20)

</style>
