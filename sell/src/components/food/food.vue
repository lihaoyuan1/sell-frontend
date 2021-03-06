<template>
  <transition name="slide">
    <div v-show="showFlag" class="food" ref="foodDetail">
      <div class="food-content">
        <div class="img-header">
          <img :src="food.image">
          <div class="back" @click="hide">
            <i class="icon-font icon-arrow_lift"></i>
          </div>
        </div>
        <div class="content">
          <h1 class="title">{{food.name}}</h1>
          <div class="detail">
            <span class="sell-count">月售{{food.sellCount}}份</span>
            <span class="rating">好评率{{food.rating}}%</span>
          </div>
          <div class="price">
            <span class="now">￥{{food.price}}</span>
            <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
          </div>
          <div class="cart-control-wrapper">
            <cart-control @cartAdd="handleCartAdd" :food="food"></cart-control>
          </div>
          <transition name="buy">
            <div @click="addFirst" class="buy" v-show="!food.count || food.count === 0">加入购物车</div>
          </transition>
        </div>
        <split v-show="food.info"></split>
        <div class="info" v-show="food.info">
          <h1 class="title">商品介绍</h1>
          <p class="text">{{food.info}}</p>
        </div>
        <split></split>
        <div class="rating">
          <h1 class="title">商品评价</h1>
          <rating-select
            :select-type="selectType"
            :only-content="onlyContent"
            :desc="desc"
            :ratins="food.ratings"
            @typeChange="handleTypeChange"
            @onlyChange="handleOnlyChange"
          ></rating-select>
          <rating-list :only-content="onlyContent" :rating-list="ratingList"></rating-list>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import Vue from 'vue'
import BScroll from 'better-scroll'
import CartControl from '../goods/cart-control/cart-control'
import Split from '../../common/split/split'
import RatingSelect from '../../common/rating-select/rating-select'
import RatingList from './rating-list/rating-list'

const POSITIVE = 0
const NEGATIVE = 1
const ALL = 2

export default {
  name: 'food',
  components: {RatingList, RatingSelect, Split, CartControl},
  props: {
    food: Object
  },
  data () {
    return {
      showFlag: false,
      selectType: ALL,
      onlyContent: false,
      desc: {
        all: '全部',
        positive: '推荐',
        negative: '吐槽'
      }
    }
  },
  methods: {
    show () {
      this.showFlag = true
      this.selectType = ALL
      this.onlyContent = false
      if (!this.scroll) {
        this.$nextTick(() => {
          this.scroll = new BScroll(this.$refs.foodDetail, {
            click: true
          })
        })
      }
    },
    hide () {
      this.showFlag = false
    },
    addFirst (event) {
      Vue.set(this.food, 'count', 1)
      this.handleCartAdd(event.target)
    },
    handleCartAdd (target) {
      this.$emit('cartAdd', target)
    },
    handleTypeChange (type) {
      this.selectType = type
    },
    handleOnlyChange (flag) {
      this.onlyContent = flag
    }
  },
  computed: {
    ratingList () {
      if (this.selectType === ALL) {
        return this.food.ratings
      } else if (this.selectType === POSITIVE) {
        return this.food.ratings.filter((rating) => {
          return rating.rateType === POSITIVE
        })
      } else if (this.selectType === NEGATIVE) {
        return this.food.ratings.filter((rating) => {
          return rating.rateType === NEGATIVE
        })
      }
    }
  }
}
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
.food
  position fixed
  left 0
  top 0
  bottom 48px
  z-index 9
  width 100%
  background-color white
  &.slide-enter-active, &.slide-leave-active
    transition all .3s linear
  &.slide-enter, &.slide-leave-to
    transform translate3d(100%, 0, 0)
  .food-content
    .img-header
      position relative
      width 100%
      height 0
      padding-bottom 100%
      img
        position absolute
        top 0
        left 0
        width 100%
        height 100%
      .back
        position absolute
        top 10px
        left 0
        .icon-arrow_lift
          display block
          padding 10px
          font-size 20px
          color white
    .content
      position relative
      padding 18px
      .title
        line-height 14px
        margin-bottom 8px
        font-size 14px
        font-weight 700
        color rgb(7, 17, 27)
      .detail
        margin-bottom 18px
        line-height 10px
        height 10px
        font-size 0
        .sell-count, .rating
          font-size 10px
          color rgb(147, 153, 159)
        .sell-count
          margin-left 12px
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
      .cart-control-wrapper
        position absolute
        right 12px
        bottom 12px
      .buy
        position absolute
        right 18px
        bottom 18px
        z-index 10
        height 24px
        line-height 24px
        padding 0 12px
        box-sizing border-box
        border-radius 12px
        font-size 10px
        color white
        background-color rgb(0, 160, 220)
        &.buy-leave-active, &.buy-enter-active
          transition all .4s
        &.buy-enter, &.buy-leave-to
          opacity 0
    .info
      padding 18px
      .title
        line-height 24px
        margin-bottom 6px
        font-size 14px
        color rgb(7, 17, 27)
      .text
        line-height 24px
        padding 0 8px
        font-size 14px
        color rgb(77, 85, 93)
    .rating
      padding-top 18px
      .title
        line-height 24px
        margin-left 18px
        font-size 14px
        color rgb(7, 17, 27)
</style>
