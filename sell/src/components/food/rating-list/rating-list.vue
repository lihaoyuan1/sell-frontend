<template>
  <div class="rating-list">
    <ul v-show="ratingList.length">
      <li v-show="needShow(rating.text)" v-for="(rating, index) in ratingList" class="rating-item border-1px" :key="index">
        <div class="user">
          <span class="name">{{rating.username}}</span>
          <img class="avatar" width="12" height="12" :src="rating.avatar">
        </div>
        <div class="time">{{rating.rateTime | _formatDate}}</div>
        <p class="text">
          <i class="icon-font" :class="rating.rateType === 0 ? 'icon-thumb_up' : 'icon-thumb_down'"></i>
          <span>{{rating.text}}</span>
        </p>
      </li>
    </ul>
    <div class="no-rating" v-show="!ratingList.length">暂无评价</div>
  </div>
</template>

<script>
import {formatDate} from '../../../assets/js/date'

export default {
  name: 'rating-list',
  props: {
    ratingList: {
      type: Array,
      default () {
        return []
      }
    },
    onlyContent: {
      type: Boolean,
      default: true
    }
  },
  methods: {
    needShow (text) {
      if (!this.onlyContent) {
        return true
      }
      return text
    }
  },
  filters: {
    _formatDate (time) {
      let date = new Date(time)
      return formatDate(date, 'yyyy-MM-dd hh:mm')
    }
  }
}
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
@import "../../../assets/stylus/mixin.styl"
.rating-list
  padding 0 18px
  .rating-item
    position relative
    padding 16px 0
    border-1px(rgba(7, 17, 27, 0.1))
    .user
      position absolute
      right 0
      top 16px
      line-height 12px
      font-size 0
      .name
        display inline-block
        margin-right 6px
        vertical-align top
        font-size 10px
        color rgb(147, 153, 159)
      .avatar
        border-radius 50%
    .time
      margin-bottom 6px
      line-height 12px
      font-size 10px
      color rgb(147, 153, 159)
    .text
      line-height 16px
      font-size 12px
      color rgb(7, 17, 27)
      .icon-thumb_up, .icon-thumb_down
        line-height 16px
        margin-right 4px
        font-size 12px
      .icon-thumb_up
        color rgb(0, 160, 220)
      .icon-thumb_down
        color rgb(147, 153, 159)
  .no-rating
    padding 16px 0
    font-size 12px
    color rgb(147, 153, 159)
</style>
