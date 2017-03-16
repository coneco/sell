<template>
  <div class="title">
    <div class="line"></div>
    <div class="text">{{content}}</div>
    <div class="line"></div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    props: {
      content: {
        type: String
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .title
    display flex
    margin 28px 36px 24px 36px
    .line
      flex 1
      position relative
      top -6px
      border-bottom 1px solid rgba(255, 255, 255, 0.2)
    .text
      padding 0 12px
      line-height 14px
      font-size 14px
      font-weight 700
      color rgb(255, 255, 255)
</style>
