<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="decreaseCart">
        <span class="inner icon-remove_circle_outline"></span>
      </div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{ food.count }}</div>
    <span class="cart-add icon-add_circle" @click.stop.prevent="addCart"></span>
  </div>
</template>

<script type="text/ecmascript-6">
import Vue from 'vue'
export default {
  props: {
    food: {
      type: Object
    }
  },
  methods: {
    addCart (event) {
      if (!this.food.count) {
        // 当我们去给一个观测对象添加一个不存在的【字段】的时候，
        // 我们不能直接这样去赋值：this.food.count = 1
        // 因为【definePropty】检测不到这个新增的属性的变化
        // 需要通过Vue.set()这种方式去给它添加一个属性
        Vue.set(this.food, 'count', 1)
      } else {
        this.food.count++
      }
      // 添加商品的同时还需要指明添加的目标target
      this.$emit('add', event.target)
    },
    decreaseCart () {
      if (this.food.count) {
        this.food.count--
      }
    }
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .cartcontrol
    font-size: 0
    .cart-decrease
      display: inline-block
      padding: 6px
      opacity: 1
      transform: translate3d(0, 0, 0)
      .inner
        display: inline-block
        line-height: 24px
        font-size: 24px
        color: rgb(0,160,220)
        transition: all 0.4s linear
        transform: rotate(0)
        /*background: green*/
      &.move-enter-active,&.move-leave-active
        transition: all 0.4s linear
      &.move-enter,&.move-leave-active
        opacity: 0
        transform: translate3d(24px, 0, 0)
        .inner
          transform: rotate(180deg)
    .cart-count
      display: inline-block
      width: 12px
      vertical-align: top
      color: rgb(147,153,159)
      line-height: 24px
      padding-top: 6px
      text-align: center
      font-size: 10px
      /*background: red*/
    .cart-add
      display: inline-block
      line-height: 24px
      padding: 6px
      font-size: 24px
      color: rgb(0,160,220)
      /*background: red*/
</style>
