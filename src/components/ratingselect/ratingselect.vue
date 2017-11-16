<template>
  <div class="ratingselect">
    <div class="rating-type">
      <span class="block positive">{{desc.all}}
        <span class="count">47</span>
      </span>
      <span class="block positive">{{desc.positive}}
        <span class="count">40</span>
      </span>
      <span class="block negative">{{desc.negative}}
        <span class="count">7</span>
      </span>
    </div>
    <div class="switch">
      <span class="icon-check_circle"></span>
      <span class="text">只看有内容的评价</span>
    </div>
  </div>
</template>

<script>
  // const POSITIVE = 0
  // const NEGATIVE = 1
  const ALL = 2

  export default {
    props: {
      ratings: {
        type: Array,
        default () {
          return []
        }
      },
      selectType: {
        type: Number,
        default: ALL
      },
      onlyContent: {
        type: Boolean,
        default: false
      },
      desc: {
        type: Object,
        default () {
          return {
            all: '全部',
            positive: '满意',
            negative: '不满意'
          }
        }
      }
    },
    data () {
      return {

      }
    }
  }
</script>

<style lang="scss" rel="stylesheet/scss">
  @import './static/css/common.scss';
  @import './src/common/sass/icon.scss';

  .ratingselect{
    .rating-type{
      padding:18px 0;
      margin:0 18px;
      @include border-bottom(rgba(7,17,27,0.1));
      font-size: 0;
      .block{
        display: inline-block;
        padding:8px 12px;
        margin-right:8px;
        border-radius: 1px;
        color:rgb(77,85,93);
        font-size: 12px;
        line-height: 16px;
        .count{
          margin-left:2px;
          font-size: 8px;

        }
        &.positive{
          background:rgba(0,160,220,0.2);
        }
        &.negative{
          background:rgba(77,85,93,0.2);
        }
      }
    }
  }
</style>