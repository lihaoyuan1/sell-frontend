<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul ref="menuList">
        <li
          v-for="(item, index) in goods"
          :key="index" class="menu-item"
          :class="{'current': menuIndex === index}"
          @click="selectMenu(index)"
        >
          <span class="text border-1px">
            <span v-show="item.type > 0" class="icon"><icon :type="item.type" :size="3"></icon></span>
            {{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodsWrapper" @touchstart="focusChange">
      <ul ref="foodList">
        <li v-for="(item, index) in goods" class="food-list" :key="index">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="(food, index1) in item.foods" class="food-item border-1px" :key="index1">
              <div class="icon">
                <img width="57" height="57" :src="food.icon">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc" v-if="food.description">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">￥{{food.price}}</span>
                  <span class="old" v-if  ="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
                <div class="control-wrapper">
                  <cart-control @cartAdd="handleCartAdd" :food="food"></cart-control>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shop-cart
      :select-foods="selectFoods"
      :delivery-price="seller.deliveryPrice"
      :min-price="seller.minPrice"
      ref="cart"
    >
    </shop-cart>
  </div>
</template>

<script>
import {GET, ERR_OK} from '../../assets/js/api'
import Icon from '../../common/icon/icon'
import ShopCart from './shop-cart/shop-cart'
import CartControl from './cart-control/cart-control'
import BScroll from 'better-scroll'

export default {
  name: 'goods',
  props: {
    seller: Object
  },
  data () {
    return {
      goods: [],
      listHeight: [],
      scrollY: 0,
      menuIndex: 0,
      isFocusRight: true
    }
  },
  computed: {
    selectFoods () {
      let foods = []
      this.goods.forEach((good) => {
        good.foods.forEach((food) => {
          if (food.count) {
            foods.push(food)
          }
        })
      })
      return foods
    }
  },
  methods: {
    handleCartAdd (target) {
      this.$refs.cart._drop(target)
    },
    focusChange () {
      if (!this.isFocusRight) {
        this.isFocusRight = true
      }
    },
    selectMenu (index) {
      if (this.menuIndex === index) {
        return
      }
      this.isFocusRight = false
      this.menuIndex = index
      let foodList = this.$refs.foodList.children
      let target = foodList[index]
      this.foodsScroll.scrollToElement(target, 300)
    },
    _initBScroll () {
      this.menuScroll = new BScroll(this.$refs.menuWrapper, {
        click: true
      })
      this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
        click: true,
        probeType: 3
      })
      this.foodsScroll.on('scroll', (pos) => {
        this.scrollY = -Math.round(pos.y)
      })
    },
    _calculateHeight () {
      let foodList = this.$refs.foodList.children
      let height = 0
      this.listHeight.push(height)
      for (let i = 0; i < foodList.length; i++) {
        let item = foodList[i]
        height += item.clientHeight
        this.listHeight.push(height)
      }
    }
  },
  watch: {
    scrollY (newVal) {
      if (!this.isFocusRight) {
        return
      }
      if (newVal < 0) {
        this.menuIndex = 0
        return
      }
      for (let i = 0; i < this.listHeight.length; i++) {
        let height = this.listHeight[i]
        if (newVal < height) {
          this.menuIndex = i - 1
          return
        }
      }
      this.menuIndex = this.listHeight.length - 1
    },
    menuIndex (newVal) {
      let menuList = this.$refs.menuList.children
      let target = menuList[newVal]
      this.menuScroll.scrollToElement(target, 300)
    }
  },
  created () {
    GET('/api/goods.json').then(res => {
      if (res.error === ERR_OK) {
        this.goods = res.data
        this.$nextTick(() => {
          this._initBScroll()
          this._calculateHeight()
        })
      }
    })
  },
  components: {
    Icon,
    ShopCart,
    CartControl
  }
}
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
@import "../../assets/stylus/mixin.styl"
.goods
  display flex
  position absolute
  top 174px
  bottom 46px
  width 100%
  overflow hidden
  .menu-wrapper
    width 80px
    background-color #f3f5f7
    .menu-item
      display table
      height 54px
      width 56px
      line-height 14px
      padding 0 12px
      &.current
        position: relative
        z-index 9
        margin-top -1px
        background-color #fff
        font-weight 700
        .text
          border-none()
      .icon
        display inline-block
        vertical-align top
        width 12px
        height 12px
      .text
        display table-cell
        width 56px
        vertical-align middle
        border-1px(rgba(7, 17, 27, 0.1))
        font-size 12px
  .foods-wrapper
    flex 1
    .title
      padding-left 14px
      height 26px
      line-height 26px
      border-left 2px solid #d9dde1
      font-size 12px
      color rgb(147, 153, 159)
      background-color #f3f5f7
    .food-item
      display flex
      margin 18px
      padding-bottom 18px
      border-1px(rgba(7, 17, 27, 0.1))
      &:last-child
        border-none()
        margin-bottom 0
      .icon
        flex 0 0 57px
        margin-right 10px
      .content
        flex 1
        .name
          margin 2px 0 8px 0
          height 14px
          line-height 14px
          font-size 14px
          color rgb(7, 17, 27)
        .desc, .extra
          line-height 10px
          font-size 10px
          color rgb(147, 153, 159)
        .desc
          margin-bottom 8px
        .extra
          .count
            margin-right 12px
        .price
          font-weight 700
          line-height 24px
          .now
            margin-right 8px
            font-size 14px
            color rgb(240 ,20 ,20)
          .old
            text-decoration line-through
            font-size 10px
            color rgb(147, 153, 159)
        .control-wrapper
          position absolute
          right 0
          bottom 12px
</style>
