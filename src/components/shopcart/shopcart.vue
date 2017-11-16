<template>
  <div class="shopcart">
    <div class="content" @click="fold=!fold">
      <div class="content-left">
        <div class="logo-wrapper">
          <div class="logo" :class="{'heightlight':totalCount>0}">
            <i class="icon-shopping_cart" :class="{'heightlight':totalCount>0}"></i>
          </div>
          <div class="num" v-show="totalCount>0">{{totalCount}}</div>
        </div>
        <div class="price" :class="{'heightlight':totalPrice>0}">￥{{totalPrice}}</div>
        <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
      </div>
      <div class="content-right" @click.stop.prevent="pay">
        <div class="pay">
          {{payDesc}}
        </div>
      </div>
    </div>
    <transition name="folder">
      <div class="shopcart-list" v-show="listShow">
        <div class="list-header">
          <h1 class="title">购物车</h1>
          <span class="empty" @click="empty">清空</span>
        </div>
        <div class="list-content" ref="list-content">
          <ul>
            <li class="food" v-for="food in selectFoods">
              <span class="name">{{food.name}}</span>
              <div class="price">
                <span>￥{{food.price*food.count}}</span>
              </div>
              <div class="cartcontrol-wrapper">
                <cartcontrol :food="food"></cartcontrol>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </transition>
    <div class="list-mask" @click="hideList" v-show="listShow"></div>
  </div>

</template>
<script>
  import cartcontrol from '../cartcontrol/cartcontrol.vue'
  import BScroll from 'better-scroll'

  export default {
    components: {
      cartcontrol
    },
    data () {
      return {
        fold: true
      }
    },
    props: {
      selectFoods: {
        type: Array,
        default () {
          return []
        }
      },
      deliveryPrice: {
        type: Number,
        default: 0
      },
      minPrice: {
        type: Number,
        default: 0
      }
    },
    computed: {
      listShow () {
        if (!this.totalCount) {
          this.fold = true
          return false
        }
        let show = !this.fold
        if (show) {
          this.$nextTick(() => {
            if (!this.scroll) {
              this.scroll = new BScroll(this.$refs['list-content'], {
                click: true
              })
            } else {
              this.scroll.refresh()
            }
          })
        }
        return show
      },
      totalPrice () {
        let total = 0
        this.selectFoods.forEach((food) => {
          total += food.price * food.count
        })
        return total
      },
      totalCount () {
        let count = 0
        this.selectFoods.forEach((food) => {
          count += food.count
        })
        return count
      },
      payDesc () {
        if (this.totalPrice === 0) {
          return `￥${this.minPrice}元起送`
        } else if (this.totalPrice < this.minPrice) {
          return `还差￥${this.minPrice - this.totalPrice}元起送`
        } else {
          return '去结算'
        }
      }
    },
    methods: {
      empty () {
        this.selectFoods.forEach((food) => {
          food.count = 0
        })
      },
      hideList () {
        this.fold = true
      },
      pay () {
        if (this.totalPrice < this.minPrice) {
          return
        }
        window.alert(`需支付${this.totalPrice}元`)
      }
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss">
  @import './static/css/common.scss';

  .shopcart{
    position: fixed;
    left:0;
    bottom:0;
    width:100%;
    z-index:50;
    height:48px;
    background:black;
    .content{
      display: flex;
      background:#141d27;
      .content-left{
        flex:1;
        .logo-wrapper{
          display: inline-block;
          position: relative;
          top:-10px;
          margin:0 12px;
          padding: 6px;
          width:56px;
          height:56px;
          box-sizing: border-box;
          vertical-align:top;
          border-radius: 50%;
          background:#141d27;
          .logo{
            width:100%;
            height:100%;
            border-radius: 50%;
            background:#2b343c;
            text-align: center;
            .icon-shopping_cart{
              font-size: 24px;
              color:#80858a;
              line-height: 44px;
              &.heightlight{
                color:white;
              }
            }
            &.heightlight{
              background:rgb(0,160,220);
            }
          }
          .num{
            position: absolute;
            top:0;
            right:0;
            width:24px;
            height:16px;
            line-height: 16px;
            text-align: center;
            border-radius: 16px;
            font-size: 19px;
            font-weight: 700;
            color:white;
            background:red;
            box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4);
          }
        }
        .price{
          display: inline-block;
          vertical-align: top;
          line-height: 24px;
          margin-top:12px;
          box-sizing: border-box;
          padding-right: 12px;
          border-right: 1px solid rgba(255,255,255,0.1);
          font-size: 16px;
          font-weight: 700;
          color:rgba(255,255,255,0.4);
          &.heightlight{
            color:white;
          }
        }
        .desc{
          display: inline-block;
          vertical-align: top;
          line-height:24px;
          margin:12px 0 0 12px;
          color:rgba(255,255,255,0.4);
          font-size: 10px;
        }
      }
      .content-right{
        flex:0 0 105px;
        width:105px;
        .pay{
          height:48px;
          line-height: 48px;
          text-align: center;
          font-size: 12px;
          color: rgba(255,255,255,0.4);
          background:#2b333b;
        }
      }

    }
    .shopcart-list{
      position: absolute;
      top:0;
      left:0;
      z-index: -1;
      width:100%;
      transition:all 0.5s;
      transform:translate3d(0,-100%,0);
      .list-header{
        height:40px;
        line-height: 40px;
        padding:0 18px;
        background:#f3f5f7;
        border-bottom: 1px solid rgba(7,17,27,0.1);
        .title{
          float:left;
          font-size: 17px;
          color:rgb(7,17,27);
        }
        .empty{
          float:right;
          font-size: 12px;
          color:rgb(0,160,220);
        }
      }
      .list-content{
        padding:0 18px;
        max-height:217px;
        background:#fff;
        overflow: hidden;
        .food{
          position: relative;
          padding: 12px 0;
          box-sizing: border-box;
          @include border-bottom(rgba(7,17,27,0.1));
          .name{
            line-height: 24px;
            font-size: 14px;
            color:rgb(7,17,27);
          }
          .price{
            position: absolute;
            right:90px;
            bottom:12px;
            line-height: 24px;
            font-size: 14px;
            font-weight: 700;
            color:rgb(240,20,20);
          }
          .cartcontrol-wrapper{
            position: absolute;
            right:0;
            bottom:6px;
          }
        }
      }
    }
  }
  .list-mask{
    position: fixed;
    top:0;
    left:0;
    width:100%;
    height:100%;
    z-index: -2;
    filter:blur(10px);
    background:rgba(7,17,27,0.6);
  }

  .folder-enter-active,.folder-leave-active{
    transition:all 0.5s;
    transform:translate3d(0,-100%,0);
  }
  .folder-enter,.folder-leave{
    transform:translate3d(0,0,0);
  }

  .mask-enter-active,.mask-leave-active{
    opacity: 1;
    background:rgba(7,17,27,0.6);
  }
  .mask-enter,.mask-leave{
    opacity: 0;
    background:rgba(7,17,27,0);
  }
</style>
