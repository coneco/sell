<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评价</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script type="text/ecmascript-6">
  import header from './components/header/header';
  
  const ERR_OK = 0;
  
  export default {
    name: 'app',
    data () {
      return {
        seller: {}
      };
    },
    created () {
      this.$http.get('/api/seller').then((response) => {
        if (response.body.errno === ERR_OK) {
          this.seller = response.body.data;
        }
      });
    },
    components: {
      'v-header': header
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "./common/stylus/mixin.styl"
  a
    text
  
  #app
    .tab
      display flex
      width 100%
      height 40px
      line-height 40px
      background-color white
      border-1px(rgba(7, 17, 27, 0.1))
      .tab-item
        flex 1
        text-align center
        & > a
          display block
          font-size 14px
          font-weight 500
          color rgb(77, 85, 93)
          &.router-link-active
            color rgb(240, 20, 20)
</style>
