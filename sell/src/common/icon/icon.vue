<template>
  <div class="icon-" :class="classMap[type]"></div>
</template>

<script>
export default {
  name: 'icon',
  props: {
    type: Number,
    size: Number
  },
  created () {
    this.classMap = ['decrease_' + this.size, 'discount_' + this.size,
      'special_' + this.size, 'invoice_' + this.size, 'guarantee_' + this.size]
  }
}
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
@import "../../assets/stylus/mixin.styl"
.icon-
  display inline-block
  width 100%
  height 100%
  background-size 100% 100%
  background-repeat no-repeat
  &.decrease_1
    bg-img('img/decrease_1')
  &.discount_1
    bg-img('img/discount_1')
  &.guarantee_1
    bg-img('img/guarantee_1')
  &.invoice_1
    bg-img('img/invoice_1')
  &.special_1
    bg-img('img/special_1')
  &.decrease_2
    bg-img('img/decrease_2')
  &.discount_2
    bg-img('img/discount_2')
  &.guarantee_2
    bg-img('img/guarantee_2')
  &.invoice_2
    bg-img('img/invoice_2')
  &.special_2
    bg-img('img/special_2')
  &.decrease_3
    bg-img('img/decrease_3')
  &.discount_3
    bg-img('img/discount_3')
  &.guarantee_3
    bg-img('img/guarantee_3')
  &.invoice_3
    bg-img('img/invoice_3')
  &.special_3
    bg-img('img/special_3')
  &.decrease_4
    bg-img('img/decrease_4')
  &.discount_4
    bg-img('img/discount_4')
  &.guarantee_4
    bg-img('img/guarantee_4')
  &.invoice_4
    bg-img('img/invoice_4')
  &.special_4
    bg-img('img/special_4')
</style>
