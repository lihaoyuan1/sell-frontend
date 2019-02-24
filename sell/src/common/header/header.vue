<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avator">
        <img :src="seller.avator" width="64px" height="64px">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="support">
          <span class="icon"><icon :type="seller.supports[0].type" :size="1"></icon></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div class="support-count" v-if="seller.supports" @click="showDetail">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-font icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="showDetail">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-font icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img :src="seller.avator" width="100%" height="100%"/>
    </div>
    <transition name="fade">
      <div class="detail" v-show="detailShow" ref="scroll">
        <div class="detail-wrapper">
          <div class="detail-main">
            <h1 class="name">{{seller.name}}</h1>
            <div class="star-wrapper">
              <star :size="48" :score="seller.score"></star>
            </div>
            <div class="title-wrapper">
              <line-title tip="优惠信息"></line-title>
            </div>
            <ul v-if="seller.supports" class="supports">
              <li class="support-item" v-for="(item, index) in seller.supports" :key="index">
                <span class="icon"><icon :type="item.type" :size="2"></icon></span>
                <span class="text">{{item.description}}</span>
              </li>
            </ul>
            <div class="title-wrapper">
              <line-title tip="商家公告"></line-title>
            </div>
            <div class="bulletin">
              <p class="content">{{seller.bulletin}}</p>
            </div>
          </div>
          <div class="detail-close" @click="showDetail">
            <i class="icon-font icon-close"></i>
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
import Star from '../star/star'
import LineTitle from '../line-title/line-title'
import Icon from '../icon/icon'
export default {
  name: 'home-header',
  props: {
    seller: Object
  },
  data () {
    return {
      detailShow: false,
      scroll: null
    }
  },
  methods: {
    showDetail () {
      this.detailShow = !this.detailShow
    }
  },
  mounted () {
    this.scroll = new BScroll(this.$refs.scroll, {
      click: true
    })
  },
  created () {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
  },
  components: {
    Star,
    LineTitle,
    Icon
  }
}
</script>

<style scoped lang="stylus" ref="stylesheet/stylus">
@import "../../assets/stylus/mixin.styl"
@import "../../assets/css/animate.css"
.header
  position relative
  color #fff
  overflow hidden
  background-color rgba(7, 17, 27, 0.5)
  .content-wrapper
    position relative
    padding 24px 12px 18px 24px
    font-size 0
    .avator
      display inline-block
      vertical-align top
      img
        border-radius 2px
    .content
      display inline-block
      margin-left 16px
      font-size 0
      .title
        margin 2px 0 8px 0
        .brand
          display inline-block
          vertical-align top
          width 30px
          height 18px
          bg-img('img/brand')
          background-size 30px 18px
          background-repeat no-repeat
        .name
          margin-left 6px
          font-size 16px
          line-height 18px
          font-weight bold
      .description
        margin-bottom 10px
        line-height 12px
        font-size 12px
      .support
        .icon
          display inline-block
          vertical-align middle
          width 12px
          height 12px
          margin-right 4px
        .text
          vertical-align middle
          line-height 12px
          font-size 12px
    .support-count
      position absolute
      right 12px
      bottom 18px
      padding 0 8px
      height 24px
      line-height 24px
      border-radius 14px
      background-color rgba(0,0,0,0.2)
      text-align center
      .count
        font-size 10px
      .icon-keyboard_arrow_right
        font-size 10px
  .bulletin-wrapper
    position relative
    height 28px
    line-height 28px
    padding 0 22px 0 12px
    white-space nowrap
    overflow hidden
    text-overflow ellipsis
    background-color rgba(7, 17, 27, 0.2)
    .bulletin-title
      display inline-block
      vertical-align middle
      width 22px
      height 12px
      bg-img('img/bulletin')
      background-size 22px 12px
      background-repeat no-repeat
    .bulletin-text
      margin 0 4px
      font-size 10px
    .icon-keyboard_arrow_right
      position absolute
      font-size 10px
      right 12px
  .background
    position absolute
    top 0
    left 0
    width 100%
    height 100%
    z-index -1
    filter blur(10px)
  .detail
    position fixed
    top 0
    left 0
    z-index 50
    width 100%
    height 100%
    overflow auto
    background-color rgba(7, 17, 27, 0.8)
    .detail-wrapper
      position relative
      min-height 100%
      .detail-main
        padding 64px 0
        .name
          line-height 16px
          text-align center
          font-size 16px
          font-weight 700
        .star-wrapper
          margin-top 18px
          padding 2px 0
          text-align center
        .title-wrapper
          width 80%
          margin 28px auto 24px auto
        .supports
          width 80%
          margin 0 auto
          .support-item
            padding 0 12px
            margin-bottom 12px
            font-size 0
            &:last-child
              margin-bottom 0
            .icon
              display inline-block
              width 16px
              height 16px
              vertical-align middle
              margin-right 6px
            .text
              vertical-align middle
              line-height 12px
              font-size 12px
        .bulletin
          width 80%
          margin 0 auto
          .content
            padding 0 12px
            line-height 24px
            font-size 12px
      .detail-close
        position absolute
        left 50%
        margin-left -16px
        bottom 24px
        width 32px
        height 32px
        font-size 32px
  .fade-enter-active
    animation: fadeInDownBig 1s ease-out
  .fade-leave-active
    animation fadeOutUpBig 1s ease-in
</style>
