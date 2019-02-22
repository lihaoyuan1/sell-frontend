<template>
  <div class="line-title">
    <div class="line"></div>
    <div class="text">{{tip}}</div>
    <div class="line"></div>
  </div>
</template>

<script>
export default {
  name: 'line-title',
  props: {
    tip: String
  }
}
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
.line-title
  display flex
  .line
    flex 1
    position relative
    top -6px
    border-bottom 1px solid rgba(255, 255, 255, 0.2)
  .text
    padding 0 12px
    font-size 14px
    font-weight 700
</style>
