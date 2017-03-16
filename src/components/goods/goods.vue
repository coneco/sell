<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuwrapper">
      <ul>
        <li class="menu-item" v-for="(good,i) in goods" :class="{'current':currentIndex===i}"
            @click="selectMenu(i, $event)">
          <div class="text border-1px">
            <icon v-show="good.type>0" :img="good.type" :tp="3" :sz="12"></icon>
            <span class="text-content">{{good.name}}</span>
          </div>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodswrapper">
      <ul>
        <li v-for="item in goods" class="food-list food-list-hook">
          <h1 class="foods-title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item border-1px">
              <div class="food-icon">
                <img :src="food.icon" alt="">
              </div>
              <div class="food-content">
                <h2 class="food-name">{{food.name}}</h2>
                <p class="food-desc">{{food.description}}</p>
                <div class="extra">
                  <span>月售{{food.sellCount}}份</span>
                  <span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">￥{{food.price}}</span>
                  <span v-show="food.oldPrice" class="old">￥{{food.oldPrice}}</span>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
  </div>
  
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll';
  import icon from '../icon/icon';
  import shopcart from '../shopcart/shopcart';
  
  const ERR_OK = 0;
  
  export default {
    name: 'goods',
    data () {
      return {
        goods: [],
        listHeight: [],
        scrollY: 0
      };
    },
    props: {
      seller: {
        type: Object
      }
    },
    computed: {
      currentIndex () {
        for (let i = 0; i < this.listHeight.length; i++) {
          let h1 = this.listHeight[i];
          let h2 = this.listHeight[i + 1];
          if (!h2 || (this.scrollY >= h1 && this.scrollY < h2)) {
            return i;
          }
        }
        return 0;
      }
    },
    created () {
      this.$http.get('api/goods').then((response) => {
        if (response.body.errno === ERR_OK) {
          this.goods = response.body.data;
          this.$nextTick(() => {
            // this DOM渲染完成后
            this._initScroll();
            this._calcHeight();
          });
        }
      });
    },
    methods: {
      selectMenu (index, event) {
        if (!event._constructed) {
          return;
        }
        let foodList = this.$refs.foodswrapper.getElementsByClassName('food-list-hook');
        let el = foodList[index];
        this.foodsScroll.scrollToElement(el, 300);
      },
      _initScroll () {
        this.menuScroll = new BScroll(this.$refs.menuwrapper, {
          click: true
        });
        this.foodsScroll = new BScroll(this.$refs.foodswrapper, {
          probeType: 3 // 监听滚动位置
        });
        this.foodsScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y));
        });
      },
      _calcHeight () {
        let foodList = this.$refs.foodswrapper.getElementsByClassName('food-list-hook');
        let height = 0;
        this.listHeight.push(height);
        for (let i = 0; i < foodList.length; i++) {
          let item = foodList[i];
          height += item.clientHeight;
          this.listHeight.push(height);
        }
      }
    },
    components: {
      icon,
      shopcart
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  
  .goods
    display flex
    position absolute
    top 174px
    width 100%
    bottom 46px
    overflow hidden
    .menu-wrapper
      flex 0 0 80px
      width 80px
      font-size 0
      background-color #f3f5f7
      .menu-item
        display table
        width 56px
        height 54px
        font-size 0
        padding 0 12px
        &.current
          position relative
          z-index 10
          margin-top -1px
          background-color #fff
          font-weight 700
          .text
            border-none()
        .icon
          margin-top 1px
          margin-right 2px
          line-height 14px
        .text
          display table-cell
          width 56px
          vertical-align middle
          font-size 0
          border-1px(rgba(7,17,27,0.1))
          .text-content
            vertical-align top
            font-size 12px
            line-height 14px
    .foods-wrapper
      flex 1
      .foods-title
        padding-left 14px
        height 26px
        line-height 26px
        font-size 12px
        color rgb(147,153,159)
        border-left 2px solid #d9dde1
        background-color #f3f5f7
      .food-item
        display flex
        padding-bottom 18px
        margin 18px
        border-1px(rgba(7,17,27,0.1))
        &:last-child
          border-none()
          padding-bottom 0
        .food-icon
          flex 0 0 57px
          margin-right 10px
          img
            height 57px
            width 57px
        .food-content
          flex 1
          .food-name
            margin-top 2px
            margin-bottom 8px
            font-size 14px
            color rgb(7,17,27)
            line-height 14px
          .food-desc
            margin-bottom 8px
            font-size 10px
            line-height 14px
            color rgb(147,153,159)
          .extra
            font-size 0
            color rgb(147,153,159)
            span
              font-size 10px
              line-height 10px
              margin-right 12px
          .price
            font-size 0
            .now
              font-size 14px
              font-weight 700
              color rgb(240,20,20)
              line-height 24px
            .old
              font-size 10px
              color rgb(147,153,159)
              line-height 24px
              text-decoration line-through
</style>
