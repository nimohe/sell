<template>
  <div class="cartcontrol">
    <transition name="move" >
      <div class="cart-decrease icon-remove_circle_outline" v-show="food.count>0" @click.stop.prevent="decreaseCart"></div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click.stop.prevent="addCart"></div>
  </div>
</template>

<script>
  import Vue from 'vue'
  export default {
    data () {
      return {}
    },
    props: {
      food: {
        type: Object
      }
    },
    created () {

    },
    methods: {
      addCart (event) {
        if (!event._constructed) {
          return
        }
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1)
        } else {
          this.food.count ++
        }
      },
      decreaseCart (event) {
        if (!event._constructed) {
          return
        }
        if (this.food.count) {
          this.food.count --
        }
      }
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss">

  .move-enter-active, .move-leave-active {
    opacity: 1;
    transform: translate3D(0, 0, 0);
  }
  .move-enter, .move-leave-to {
    opacity: 0;
    transform: translate3D(24px, 0, 0) rotate(180deg);
  }

  .cartcontrol{
    font-size: 0;
    .cart-decrease{
      display: inline-block;
      padding:6px;
      font-size: 24px;
      line-height: 24px;
      color:rgb(0,160,220);
      transition:all 0.2s linear;
    }
    .cart-count{
      display: inline-block;
      vertical-align: top;
      width: 12px;
      padding-top: 6px;
      line-height: 24px;
      text-align: center;
      font-size: 10px;
      color:rgb(147, 153, 159);
    }
    .cart-add{
      display: inline-block;
      font-size: 24px;
      line-height: 24px;
      padding:6px;
      color:rgb(0,160,220);
    }
  }
</style>
