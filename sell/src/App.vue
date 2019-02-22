<template>
  <div id="app">
    <sell-header :seller="seller"></sell-header>
    <sell-tab></sell-tab>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
import SellHeader from './common/header/header'
import SellTab from './common/tab/tab'
import {GET, ERR_OK} from './assets/js/api'

export default {
  name: 'App',
  components: {SellHeader, SellTab},
  data () {
    return {
      seller: {}
    }
  },
  created () {
    GET('/api/seller.json').then(res => {
      if (res.error === ERR_OK) {
        this.seller = res.data
      }
    })
  }
}
</script>

<style>
</style>
