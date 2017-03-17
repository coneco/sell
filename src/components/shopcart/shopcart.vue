<template>
  <div class="shopcart">
    <div class="content">
      <div class="content-left">
        <div class="logo-wrapper">
          <div class="logo" :class="{'highlight':totalCount>0}">
            <i class="icon-shopping_cart" :class="{'highlight':totalCount>0}"></i>
          </div>
          <div class="num" v-show="totalCount>0">{{totalCount}}</div>
        </div>
        <div class="price border-1px-right" :class="{'highlight':totalPrice>0}">￥{{totalPrice}}</div>
        <div class="desc">另需配送费{{deliveryPrice}}元</div>
      </div>
      <div class="content-right">
        <div class="pay" :class="{'highlight':totalPrice>=minPrice}">{{payStatus}}</div>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    props: {
      deliveryPrice: {
        type: Number,
        default: 0
      },
      minPrice: {
        type: Number,
        default: 0
      },
      selectedFoods: {
        type: Array,
        default () {
          return [{
            price: 0,
            count: 1
          }];
        }
      }
    },
    computed: {
      totalPrice () {
        let total = 0;
        this.selectedFoods.forEach((food) => {
          total += food.price * food.count;
        });
        return total;
      },
      totalCount () {
        let cnt = 0;
        this.selectedFoods.forEach((food) => {
          cnt += food.count;
        });
        return cnt;
      },
      payStatus () {
        if (this.totalPrice === 0) {
          return `￥${this.minPrice}元起送`;
        } else if (this.totalPrice < this.minPrice) {
          let diff = this.minPrice - this.totalPrice;
          return `还差${diff}元起送`;
        } else {
          return '去结算';
        }
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  
  .shopcart
    position fixed
    left 0
    bottom 0
    z-index 50
    width 100%
    height 48px
    font-size 0
    .content
      display flex
      background-color #141d27
      .content-left
        flex 1
        .logo-wrapper
          display inline-block
          position relative
          top -10px
          margin 0 12px
          padding 6px
          height 56px
          width 56px
          box-sizing border-box
          vertical-align top
          border-radius 50%
          background-color #141D27
          .logo
            width 100%
            height 100%
            border-radius 50%
            text-align center
            background-color #2B343C
            &.highlight
              background-color rgb(0, 160, 220)
            .icon-shopping_cart
              font-size 24px
              line-height 44px
              color #80858A
              &.highlight
                color #FFF
          .num
            position absolute
            top 0
            right 0
            width 24px
            height 16px
            line-height 16px
            text-align center
            border-radius 6px
            font-size 9px
            font-weight 700
            color #FFF
            background-color rgb(240, 20, 20)
            box-shadow 0 4px 8px 0px rgba(0, 0, 0, 0.4)
        .price
          display inline-block
          vertical-align top
          line-height 24px
          margin-top 12px
          padding-right 12px
          box-sizing border-box
          border-1px-right(rgba(255, 255, 255, 0.1))
          font-size 16px
          font-weight 700
          color rgba(255, 255, 255, 0.4)
          &.highlight
            color #FFF
        .desc
          display inline-block
          vertical-align top
          line-height 24px
          margin 12px 0 0 12px
          box-sizing border-box
          font-size 10px
          color rgba(255, 255, 255, 0.4)
      .content-right
        flex 0 0 105px
        width 105px
        .pay
          height 48px
          line-height 48px
          text-align center
          font-size 12px
          font-weight 700
          color rgba(255, 255, 255, 0.4)
          background-color #2B333B
          &.highlight
            color #FFF
            background-color #00B43C
</style>
