<template>
  <div class="tab border-1px">
    <router-link tag="div" to="goods" class="tab-item">商品</router-link>
    <router-link tag="div" to="ratings" class="tab-item">评论</router-link>
    <router-link tag="div" to="seller" class="tab-item">商家</router-link>
  </div>
</template>

<script>
export default {
  name: 'tab'
}
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
@import "../../assets/stylus/mixin.styl"
.tab
  width 100%
  display flex
  flex-direction row
  height 40px
  line-height 40px
  border-1px(rgba(7, 17, 21, 0.1))
  .tab-item
    flex 1
    text-align center
    font-size 14px
    color rgba(77, 85, 93, 1)
    &.v-active
      color rgba(240, 20, 20, 1)
</style>
