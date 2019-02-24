<template>
  <div class="seller" ref="seller">
    <div class="seller-content">
      <div class="over-view">
        <h1 class="title">{{seller.name}}</h1>
        <div class="desc border-1px">
          <star class="star" :size="36" :score="seller.score"></star>
          <span class="text">({{seller.ratingCount}})</span>
          <span class="text">月售{{seller.sellCount}}单</span>
        </div>
        <ul class="remark">
          <li class="block">
            <h2>起送价</h2>
            <div class="content">
              <span class="stress">{{seller.minPrice}}元</span>
            </div>
          </li>
          <li class="block">
            <h2>商家配送</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryPrice}}元</span>
            </div>
          </li>
          <li class="block">
            <h2>平均配送时间</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryTime}}分钟</span>
            </div>
          </li>
        </ul>
        <div class="favorite" @click="toggleFavorite">
          <span class="icon-font icon-favorite" :class="{'active': favorite}"></span>
          <span class="text">{{favoriteText}}</span>
        </div>
      </div>
      <split></split>
      <div class="bulletin">
        <h1 class="title">公告与活动</h1>
        <div class="content-wrapper border-1px">
          <p class="content">{{seller.bulletin}}</p>
        </div>
        <ul v-if="seller.supports" class="supports">
          <li class="support-item border-1px" v-for="(item, index) in seller.supports" :key="index">
            <span class="icon"><icon :type="item.type" :size="4"></icon></span>
            <span class="text">{{item.description}}</span>
          </li>
        </ul>
      </div>
      <split></split>
      <div class="pics">
        <h1 class="title">商家实景图</h1>
        <div class="pic-wrapper" ref="picWrapper">
          <ul class="pic-list" ref="picList">
            <li class="pic-item" v-for="(pic, index) in seller.pics" :key="index">
              <img @load="_initSlider" width="120" height="90" :src="pic"/>
            </li>
          </ul>
        </div>
      </div>
      <split></split>
      <div class="info">
        <h1 class="title border-1px">商家信息</h1>
        <ul>
          <li class="info-item border-1px" v-for="(info, index) in seller.infos" :key="index">{{info}}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
import Star from '../../common/star/star'
import Split from '../../common/split/split'
import Icon from '../../common/icon/icon'
export default {
  name: 'seller',
  components: {Icon, Split, Star},
  props: {
    seller: Object
  },
  data () {
    return {
      favorite: false
    }
  },
  methods: {
    toggleFavorite () {
      this.favorite = !this.favorite
    },
    _initSlider () {
      if (!this.slider) {
        this.$refs.picList.style.width = `${this.seller.pics.length * 130 - 10}px`
        this.slider = new BScroll(this.$refs.picWrapper, {
          scrollX: true,
          scrollY: false
        })
      }
    }
  },
  computed: {
    favoriteText () {
      return this.favorite ? '已收藏' : '收藏'
    }
  },
  mounted () {
    this.scroll = new BScroll(this.$refs.seller, {
      click: true
    })
  }
}
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
@import "../../assets/stylus/mixin.styl"
.seller
  position absolute
  top 174px
  bottom 0
  left 0
  width 100%
  overflow hidden
  .over-view
    position relative
    padding 18px
    .title
      margin-bottom 8px
      line-height 14px
      color rgb(7, 17, 27)
      font-size 14px
    .desc
      padding-bottom 18px
      line-height 18px
      border-1px(rgba(7, 17, 27, 0.1))
      font-size 0
      .star
        display inline-block
        margin-right 8px
        vertical-align top
      .text
        margin-right 12px
        display inline-block
        vertical-align top
        line-height 18px
        font-size 10px
        color rgb(147, 153, 159)
    .remark
      display flex
      padding-top 18px
      .block
        flex 1
        text-align center
        border-right 1px solid rgba(7, 17, 27, 0.1)
        &:last-child
          border none
        h2
          margin-bottom 4px
          line-height 10px
          font-size 10px
          color rgb(147, 153, 159)
        .content
          line-height 24px
          font-size 10px
          color rgb(7, 17, 27)
          .stress
            font-size 24px
    .favorite
      position absolute
      right 18px
      top 18px
      text-align center
      width 38px
      .icon-favorite
        display block
        margin-bottom 4px
        line-height 24px
        font-size 24px
        color #d4d6d9
        &.active
          color rgb(240, 20, 20)
      .text
        line-height 10px
        font-size 10px
        color rgb(147, 153, 159 )
  .bulletin
    padding 18px 18px 0 18px
    .title
      margin-bottom 8px
      line-height 14px
      color rgb(7, 17, 27)
      font-size 14px
    .content-wrapper
      padding 0 12px 16px 12px
      border-1px(rgba(7, 17, 27, 0.1))
      .content
        line-height 24px
        font-size 12px
        color rgb(240, 20, 20)
    .supports
      .support-item
        padding 12px 16px
        border-1px(rgba(7, 17, 27, 0.1))
        font-size 0
        &:last-child
          border-none()
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
  .pics
    padding 18px
    .title
      margin-bottom 12px
      line-height 14px
      color rgb(7, 17, 27)
      font-size 14px
    .pic-wrapper
      width 100%
      overflow hidden
      white-space nowrap
      .pic-list
        font-size 0
        .pic-item
          display inline-block
          margin-left 10px
          width 120px
          height 90px
          &:first-child
            margin-left 0
  .info
    padding 18px 18px 0 18px
    .title
      padding-bottom 12px
      line-height 14px
      border-1px(rgba(7, 17, 27, 0.1))
      color rgb(7, 17, 27)
      font-size 14px
    .info-item
      padding 16px 12px
      line-height 16px
      font-size 12px
      border-1px(rgba(7, 17, 27, 0.1))
      &:last-child
        border-none()
</style>
