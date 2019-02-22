<template>
  <div class="cart-control">
    <transition name="fade1">
      <div class="cart-decrease" v-show="food.count > 0" @click="decreaseCart">
        <i class="icon-font icon-remove_circle_outlin"></i>
      </div>
    </transition>
    <div class="cart-count" v-show="food.count > 0">{{food.count}}</div>
    <div class="cart-add" @click="addCart">
      <i class="icon-font icon-add_circle"></i>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
export default {
  name: 'cart-control',
  props: {
    food: {
      type: Object
    }
  },
  methods: {
    decreaseCart () {
      if (this.food.count) {
        this.food.count--
      }
    },
    addCart (event) {
      if (!this.food.count) {
        Vue.set(this.food, 'count', 1)
        this.food.count = 1
      } else {
        this.food.count++
      }
      this.$emit('cartAdd', event.target)
    }
  }
}
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
.cart-control
  font-size 0
  .cart-decrease
    .icon-font
      display inline-block
    &.fade1-enter, &.fade1-leave-to
      opacity 0
      transform translate3d(24px, 0, 0)
      .icon-font
        transform rotate(180deg)
    &.fade1-enter-active, &.fade1-leave-active
      transition all 0.4s linear
      .icon-font
        transition all 0.4s linear
  .cart-decrease, .cart-add
    display inline-block
    line-height 24px
    font-size 24px
    padding 6px
    color rgb(0, 160, 220)
  .cart-count
    display inline-block
    vertical-align top
    width 12px
    padding-top 6px
    line-height 24px
    text-align center
    font-size 10px
    color rgb(147, 153, 159)
</style>
