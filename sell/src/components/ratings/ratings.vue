<template>
  <div class="ratings" ref="ratings">
    <div class="ratings-content">
      <div class="over-view">
        <div class="over-view-left">
          <h1 class="score">{{seller.score}}</h1>
          <div class="title">综合评分</div>
          <div class="rank">高于周边商家{{seller.rankRate}}%</div>
        </div>
        <div class="over-view-right">
          <div class="score-wrapper">
            <span class="title">服务态度</span>
            <star :size="36" :score="seller.serviceScore"></star>
            <span class="score">{{seller.serviceScore}}</span>
          </div>
          <div class="score-wrapper">
            <span class="title">商品评分</span>
            <star class="star" :size="36" :score="seller.foodScore"></star>
            <span class="score">{{seller.foodScore}}</span>
          </div>
          <div class="delivery-wrapper">
            <span class="title">送达时间</span>
            <span class="time">{{seller.deliveryTime}}分钟</span>
          </div>
        </div>
      </div>
      <split></split>
      <rating-select
        :select-type="selectType"
        :only-content="onlyContent"
        :ratins="ratings"
        @typeChange="handleTypeChange"
        @onlyChange="handleOnlyChange"
      >
      </rating-select>
      <div class="rating-wrapper">
        <ul>
          <li
            v-for="(rating, index) in ratingList"
            class="rating-item border-1px"
            :key="index"
            v-show="needShow(rating.text)"
          >
            <div class="avatar">
              <img width="28" height="28" :src="rating.avatar"/>
            </div>
            <div class="content">
              <h1 class="name">{{rating.username}}</h1>
              <div class="star-wrapper">
                <star :size="24" :score="rating.score"></star>
                <span class="delivery" v-show="rating.deliveryTime">{{rating.deliveryTime}}分钟送达</span>
              </div>
              <p class="text">{{rating.text}}</p>
              <div class="recommend" v-show="rating.recommend && rating.recommend.length">
                <span class="icon-font icon-thumb_up"></span>
                <span class="item" v-for="(item, index) in rating.recommend" :key="index">{{item}}</span>
              </div>
              <div class="time">{{rating.rateTime | _formatDate}}</div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import BScroll from 'better-scroll'
import {GET, ERR_OK} from '../../assets/js/api'
import Star from '../../common/star/star'
import Split from '../../common/split/split'
import RatingSelect from '../../common/rating-select/rating-select'
import {formatDate} from '../../assets/js/date'

const POSITIVE = 0
const NEGATIVE = 1
const ALL = 2

export default {
  name: 'ratings',
  components: {RatingSelect, Split, Star},
  props: {
    seller: Object
  },
  data () {
    return {
      ratings: [],
      selectType: ALL,
      onlyContent: false
    }
  },
  computed: {
    ratingList () {
      if (this.selectType === ALL) {
        return this.ratings
      } else if (this.selectType === POSITIVE) {
        return this.ratings.filter((rating) => {
          return rating.rateType === POSITIVE
        })
      } else if (this.selectType === NEGATIVE) {
        return this.ratings.filter((rating) => {
          return rating.rateType === NEGATIVE
        })
      }
    }
  },
  methods: {
    needShow (text) {
      if (!this.onlyContent) {
        return true
      }
      return text
    },
    handleTypeChange (type) {
      this.selectType = type
    },
    handleOnlyChange (flag) {
      this.onlyContent = flag
    }
  },
  filters: {
    _formatDate (time) {
      let date = new Date(time)
      return formatDate(date, 'yyyy-MM-dd hh:mm')
    }
  },
  created () {
    GET('/api/ratings').then(res => {
      if (res.error === ERR_OK) {
        this.ratings = res.data
        this.$nextTick(() => {
          this.scroll = new BScroll(this.$refs.ratings, {
            click: true
          })
        })
      }
    })
  }
}
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
@import "../../assets/stylus/mixin.styl"
.ratings
  position absolute
  top 174px
  bottom 0
  left 0
  width 100%
  overflow hidden
  .over-view
    display flex
    padding 18px 0
    .over-view-left
      flex 0 0 137px
      width 137px
      padding 6px 0
      border-right 1px solid rgba(7, 17, 27, 0.1)
      text-align center
      @media only screen and (max-width: 375px)
        flex 0 0 110px
        width 110px
      .score
        margin-bottom 6px
        line-height 28px
        font-size 24px
        color rgb(255, 153, 0)
      .title
        margin-bottom 8px
        line-height 12px
        font-size 12px
        color rgb(7, 17, 27)
      .rank
        line-height 10px
        font-size 10px
        color rgb(147, 153, 159)
    .over-view-right
      flex 1
      padding 6px 0 6px 24px
      @media only screen and (max-width: 375px)
        padding 6px
      .score-wrapper
        line-height 18px
        margin-bottom 8px
        font-size 0
        .title
          display inline-block
          vertical-align top
          font-size 12px
          color rgb(7, 17, 27)
        .star
          display inline-block
          vertical-align top
          margin 0 12px
        .score
          display inline-block
          line-height 18px
          vertical-align top
          font-size 12px
          color rgb(255, 130, 0)
      .delivery-wrapper
        font-size 0
        .title
          font-size 12px
          color rgb(7, 17, 27)
        .time
          margin-left 12px
          font-size 12px
          color rgba(7, 17, 27, 0.5)
  .rating-wrapper
    padding 0 18px
    .rating-item
      display flex
      padding 18px 0
      border-1px(rgba(7, 17, 27, 0.1))
      .avatar
        flex 0 0 28px
        width 28px
        margin-right 12px
        img
          border-radius 50%
      .content
        position relative
        flex 1
        .name
          margin-bottom 4px
          line-height 12px
          font-size 10px
          color rgb(7, 17, 27)
        .star-wrapper
          margin-bottom 6px
          font-size 0
          .star
            display inline-block
            margin-right 6px
            vertical-align top
          .delivery
            display inline-block
            vertical-align top
            font-size 10px
            color rgba(7, 17, 27, 0.5)
        .text
          margin-bottom 8px
          line-height 18px
          color rgb(7, 17, 27)
          font-size 12px
        .recommend
          line-height 16px
          font-size 0
          .icon-thumb_up, .item
            display inline-block
            vertical-align top
            margin 0 8px 4px 0
            font-size 9px
          .icon-thumb_up
            color rgb(0, 160, 220)
          .item
            padding 0 6px
            border 1px solid rgba(7, 17, 27, 0.1)
            border-radius 1px
        .time
          position absolute
          top 0
          right 0
          color rgba(7, 17, 27, 0.5)
          font-size 10px
</style>
