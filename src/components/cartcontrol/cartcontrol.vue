<template>
  <div class="cartcontrol">
      <!-- v-show="food.count>0" -->
   <div class="cart-decrease" v-show="food.count>0" @click="decreaseCart"><img src="../../../resource/img/descrease.png" width="18" height="18"></div>
   <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
   <div class="cart-add" @click="addCart"><img src="../../../resource/img/add.png"  width="18" height="18"></div>
  </div>
</template>

<script>
import Vue from 'vue';
export default {
  props: {
    food: {
      type: Object
    }
  },
  created() {},
  methods: {
    addCart(event) {
      if (!event._constructed) {
        return;
      }
      if (!this.food.count) {
        Vue.set(this.food,'count',1);
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
</style>