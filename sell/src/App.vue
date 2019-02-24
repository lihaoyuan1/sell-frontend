<template>
  <div id="app">
    <sell-header :seller="seller"></sell-header>
    <sell-tab></sell-tab>
    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>
  </div>
</template>

<script>
import SellHeader from './common/header/header'
import SellTab from './common/tab/tab'
import {GET, ERR_OK} from './assets/js/api'
import {urlParse} from './assets/js/util'

export default {
  name: 'App',
  components: {SellHeader, SellTab},
  data () {
    return {
      seller: {
        id: (() => {
          let queryParam = urlParse()
          return queryParam.id
        })()
      }
    }
  },
  created () {
    GET('/api/seller').then(res => {
      if (res.error === ERR_OK) {
        this.seller = Object.assign({}, this.seller, res.data)
      }
    })
  }
}
</script>

<style>
</style>
