<template>
  <div class="cartcontrol">
      <!-- v-show="food.count>0" -->  
      <transition name="move">
        <div class="cart-decrease" v-show="food.count>0" @click="decreaseCart" >
          <img src="../../../resource/img/descrease.png" width="18" height="18" class="inner">        
        </div>  
      </transition>
      <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
      <div class="cart-add" @click="addCart"><img src="../../../resource/img/add.png"  width="18" height="18"></div>
  </div>
</template>

<script>
import Vue from "vue";

export default {
  props: {
    food: {
      type: Object
    }
  },
  methods: {
    addCart(event) {
      if (!event._constructed) {
        return;
      }
      if (!this.food.count) {
        Vue.set(this.food, "count", 1);
      } else {
        this.food.count++;
      }
    },
    decreaseCart(event) {
      if (!event._constructed) {
        return;
      }
      if (this.food.count) {
        this.food.count--;
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.cart-decrease,
.cart-add {
  display: inline-block;
  padding: 6px;
  vertical-align: middle;
}
.cart-count {
  display: inline-block;
  width: 12px;
  font-size: 10px;
  color: rgb(147, 153, 159);
}
.cart-add {
  display: inline-block;
}
/* 动画效果好像没出来 */
.cart_decrease {
  transition: all 0.4s linear;
}
.inner {
  transition: all 0.4s linear;
}
.move-enter-active {
  opacity:1 ;     
  transform:translate3d(0,0,0) 
}
/* .move-enter-active  */
.inner {
  transform: rotate(0);
}
.move-enter,
.move-leave-to {
  opacity: 0;
  transform: translate3d(24px, 0, 0);
}
/* .move-enter .inner,
.move-leave-to */
.inner {
  transform: rotate(180deg);
}
</style>