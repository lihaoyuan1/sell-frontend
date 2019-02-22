<template>
  <div>
    <div class="shop-cart">
      <div class="content" @click="toggleList">
        <div class="content-left">
          <div class="logo-wrapper">
            <div class="logo" :class="{'high-light': totalCount > 0}">
              <i class="icon-font icon-shopping_cart" :class="{'high-light': totalCount > 0}"></i>
            </div>
            <div class="num" v-show="totalCount > 0">{{totalCount}}</div>
          </div>
          <div class="price" :class="{'high-light': totalPrice > 0}">￥{{totalPrice}}</div>
          <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
        </div>
        <div class="content-right">
          <div class="pay" :class="payClass" @click.stop="pay">{{payDesc}}</div>
        </div>
      </div>
      <div class="ball-container">
        <template v-for="(ball, index) in balls">
          <transition
            name="ball"
            :key="index"
            @before-enter="beforeEnter"
            @enter="Enter"
            @after-enter="afterEnter"
          >
            <div v-show="ball.show" class="ball">
              <div class="inner"></div>
            </div>
          </transition>
        </template>
      </div>
      <transition name="fade">
        <div class="shop-cart-list" v-show="listShow">
          <div class="list-header">
            <h1 class="title">购物车</h1>
            <span class="empty" @click="empty">清空</span>
          </div>
          <div class="list-content" ref="scroll">
            <ul>
              <li class="food" v-for="(food, index) in selectFoods" :key="index">
                <span class="name">{{food.name}}</span>
                <div class="price">
                  <span>￥{{food.price * food.count}}</span>
                </div>
                <div class="cart-control-wrapper">
                  <cart-control @cartAdd="_drop" :food="food"></cart-control>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </transition>
    </div>
    <transition name="mask">
      <div class="list-mask" @click="toggleList" v-show="listShow"></div>
    </transition>
  </div>
</template>

<script>
import CartControl from '../cart-control/cart-control'
import BScroll from 'better-scroll'
export default {
  name: 'shop-cart',
  components: {CartControl},
  props: {
    selectFoods: {
      type: Array,
      default () {
        return []
      }
    },
    deliveryPrice: {
      type: Number,
      default: 0
    },
    minPrice: {
      type: Number,
      default: 0
    }
  },
  data () {
    return {
      balls: [
        {show: false},
        {show: false},
        {show: false},
        {show: false},
        {show: false}
      ],
      dropBalls: [],
      fold: false
    }
  },
  methods: {
    beforeEnter (el) {
      let count = this.balls.length
      while (count--) {
        let ball = this.balls[count]
        if (ball.show) {
          let rect = ball.el.getBoundingClientRect()
          let x = rect.left - 32
          let y = -(window.innerHeight - rect.top - 22)
          el.style.display = ''
          el.style.webkitTransform = `translate3d(0,${y}px,0)`
          el.style.transform = `translate3d(0,${y}px,0)`
          el.firstElementChild.style.webkitTransform = `translate3d(${x}px,0,0)`
          el.firstElementChild.style.transform = `translate3d(${x}px,0,0)`
        }
      }
    },
    Enter (el) {
      // eslint-disable-next-line
      let rf = el.offsetHeight
      el.style.webkitTransform = `translate3d(0,0,0)`
      el.style.transform = `translate3d(0,0,0)`
      el.firstElementChild.style.webkitTransform = `translate3d(0,0,0)`
      el.firstElementChild.style.transform = `translate3d(0,0,0)`
    },
    afterEnter (el) {
      let ball = this.dropBalls.shift()
      if (ball) {
        ball.show = false
        el.style.display = ''
      }
    },
    _drop (target) {
      for (let i = 0; i < this.balls.length; i++) {
        let ball = this.balls[i]
        if (!ball.show) {
          ball.show = true
          ball.el = target
          this.dropBalls.push(ball)
          return
        }
      }
    },
    toggleList () {
      if (!this.totalCount) {
        return
      }
      this.fold = !this.fold
    },
    empty () {
      this.selectFoods.forEach((food) => {
        food.count = 0
      })
    },
    pay () {
      if (this.totalPrice < this.minPrice) {
        return
      }
      window.alert(`支付${this.totalPrice}`)
    }
  },
  computed: {
    totalPrice () {
      let total = 0
      this.selectFoods.forEach((food) => {
        total += food.price * food.count
      })
      return total
    },
    totalCount () {
      let count = 0
      this.selectFoods.forEach((food) => {
        count += food.count
      })
      return count
    },
    payDesc () {
      if (this.totalPrice === 0) {
        return `￥${this.minPrice}元起送`
      } else if (this.totalPrice < this.minPrice) {
        let diff = this.minPrice - this.totalPrice
        return `还差￥${diff}元起送`
      } else {
        return '去结算'
      }
    },
    payClass () {
      if (this.totalPrice < this.minPrice) {
        return 'not-enough'
      } else {
        return 'enough'
      }
    },
    listShow () {
      if (!this.totalCount) {
        // eslint-disable-next-line
        this.fold = true
        return false
      }
      let show = !this.fold
      if (show && !this.scroll) {
        // eslint-disable-next-line
        this.scroll = new BScroll(this.$refs.scroll, {
          click: true
        })
      }
      return show
    }
  }
}
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
@import "../../../assets/stylus/mixin.styl"
.shop-cart
  position fixed
  left 0
  bottom 0
  z-index 11
  width 100%
  height 48px
  .content
    display flex
    background-color #141d27
    color rgba(255, 255, 255, 0.4)
    .content-left
      flex 1
      font-size 0
      .logo-wrapper
        display inline-block
        position relative
        top -10px
        margin 0 12px
        padding 6px
        width 56px
        height 56px
        box-sizing border-box
        vertical-align top
        border-radius 28px
        background-color #141d27
        .logo
          width 100%
          height 100%
          border-radius 50%
          background-color #2b343c
          text-align center
          &.high-light
            background-color rgb(0, 160, 220)
          .icon-shopping_cart
            line-height 44px
            font-size 24px
            color: #80858a
            &.high-light
              color white
        .num
          position absolute
          top 0
          right 0
          width 24px
          height 16px
          line-height 16px
          text-align center
          border-radius 16px
          font-size 9px
          font-weight 400
          color white
          background-color rgb(240, 20, 20)
          box-shadow 0 4px 8px 0 rgba(0, 0, 0, 0.4)
      .price
        display inline-block
        vertical-align top
        margin-top 12px
        line-height 24px
        box-sizing border-box
        padding-right 12px
        border-right 1px solid rgba(255, 255, 255, 0.1)
        font-size 16px
        font-weight 700
        &.high-light
          color white
      .desc
        display inline-block
        vertical-align top
        line-height 24px
        margin 12px 0 0 12px
        font-size 12px
    .content-right
      flex 0 0 105px
      width 105px
      .pay
        height 48px
        line-height 48px
        text-align center
        font-size 12px
        font-weight 700
        &.not-enough
          background-color #2b343c
        &.enough
          background-color #00b43c
          color white
  .ball-container
    .ball
      position fixed
      left 32px
      bottom 22px
      z-index 20
      &.ball-enter-active
        transition all 0.4s cubic-bezier(0.49, -0.29, 0.75, 0.41)
        .inner
          width 16px
          height 16px
          border-radius 50%
          background-color rgb(0, 160, 220)
          transition all 0.4s linear
  .shop-cart-list
    position absolute
    left 0
    top 0
    z-index -1
    width 100%
    transform translate3d(0, -100%, 0)
    &.fade-enter, &.fade-leave-to
      transform translate3d(0, 0, 0)
    &.fade-enter-active, &.fade-leave-active
      transition all .5s
    .list-header
      height 40px
      line-height 40px
      padding 0 18px
      background-color #f3f5f7
      border-bottom 1px solid rgba(7, 17, 27, 0.1)
      .title
        float left
        font-size 14px
        color rgb(7,17, 27)
      .empty
        float right
        font-size 12px
        color rgb(0, 160, 200)
    .list-content
      padding 0 18px
      max-height 217px
      overflow hidden
      background-color white
      .food
        position relative
        padding 12px 0
        box-sizing border-box
        border-1px(rgba(7, 17, 27, 0.1))
        .name
          line-height 24px
          font-size 14px
          color rgb(7, 17, 27)
        .price
          position absolute
          right 90px
          bottom 12px
          line-height 24px
          font-size 14px
          font-weight 700
          color rgb(240, 20, 20)
        .cart-control-wrapper
          position absolute
          right 0
          bottom 6px
.list-mask
  position fixed
  top 0
  left 0
  width 100%
  height 100%
  z-index 10
  background-color rgba(7, 17, 27, 0.6)
  &.mask-enter-active, &.mask-leave-active
    transition all .5s
  &.mask-enter, &.mask-leave-to
    opacity 0
    background-color rgba(7, 17, 27, 0)
</style>
