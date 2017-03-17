<template>
  <div class="cartcontrol">
    <div class="cart-remove icon-remove_circle_outline" v-show="food.count>0" @click="removeCart"></div>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click="addCart"></div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue';
  
  export default {
    props: {
      food: {
        type: Object
      }
    },
    created () {
      console.log(this.food);
    },
    methods: {
      addCart (event) {
        if (!event._constructed) {
          return;
        }
        if (this.food.count) {
          this.food.count++;
        } else {
          Vue.set(this.food, 'count', 1);
        }
      },
      removeCart (event) {
        if (!event._constructed) {
          return;
        }
        this.food.count--;
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .cartcontrol
    font-size 0
    .cart-remove, .cart-add
      display inline-block
      vertical-align top
      padding 6px
      line-height 24px
      font-size 24px
      color rgb(0, 160, 220)
    .cart-count
      display inline-block
      vertical-align top
      text-align center
      width 18px
      padding 6px 0
      line-height 24px
      font-size 10px
      color rgb(147, 153, 159)
</style>
