<template>
  <div id="app">
      <v-header :seller="seller"></v-header>
      <div class="tab border-1px">
        <div class="tab-item">
          <router-link to="/goods" active-class="active">商品</router-link>
        </div>
        <div class="tab-item">
          <router-link to="/ratings" active-class="active">评论</router-link>
        </div>
        <div class="tab-item">
          <router-link to="/seller" active-class="active">商家</router-link>
        </div>
      </div>
      <keep-alive> //保留组件的状态
        <router-view :seller="seller"></router-view>
      </keep-alive>
  </div>
</template>

<script type="text/ecmascript-6">
import {urlParse} from './common/js/util'
import header from './components/header/Header.vue'
const ERR_OK = 0

export default {
    components: {
      'v-header': header
    },
    data() {
      return {
        seller: {
          id:(() =>{ //立即执行函数，拿到url参数
            let queryParam = urlParse()
            return queryParam.id
            console.log(queryParam)
          })()
        }
      }
    },
    created() {
        var _this=this
        this.$http.get('/api/seller?id=' + this.seller.id).then((res) => {
        /*_this.seller = res.data.data*/
        _this.seller = Object.assign({}, this.seller, res.data.data)//给已有对象扩展属性
        console.log(_this.seller.id)
      }).catch(function(err){
        console.log(err)
      })
    }
}
</script>

<style lang="stylus" rel="stylesheet/stylus" >
  @import "../src/common/stylus/mixin.styl"
  .tab
    display: flex
    width: 100%
    height: 40px
    line-height: 40px
    border-1px(rgba(7, 17, 27, 0.1))
    .tab-item
      flex: 1
      text-align: center
      & > a
        display: block
        font-size: 14px
        color: rgb(77, 85, 93)
        &.active
          color: rgb(240, 20, 20)

</style>
