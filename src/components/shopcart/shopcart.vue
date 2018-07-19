<template>
  <div class="shopcart">
   <div class="content">
     <div class="content-left">
       <div class="logo-wrapper">
         <div class="logo" :class="{'hightlight':totalCount>0}">
           <img src="../../../resource/img/shopcart.png" class="icon-shopping-cart" v-show="totalCount>0">
            <img src="../../../resource/img/shopcart1.png" class="icon-shopping-cart" v-show="totalCount==0">
         </div>
         <div class="number" v-show="totalCount>0">{{totalCount}}</div>
       </div>
       <div class="price" :class="{'pricehightlight':totalPrice>0}">￥{{totalPrice}}</div>
       <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
     </div>
      <div class="content-right">
        <div class="pay" :class="payClass">
          {{payDesc}}
        </div>
      </div>
   </div>
  <div class="shopcart-list" v-show="listShow">
    <div class="list-header">
      <h1 class="title">购物车</h1>
      <span class="empty">清空</span>
    </div>
    <div class="list-content">
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
  </div>
</template>

<script>
export default {
  props: {
    selectFoods: {
      type: Array,
      default() {
        return [];
      }
    },
    deliveryPrice: {
      type: Number,
      default: 0 //默认值
    },
    minPrice: {
      type: Number,
      default: 0 //默认值
    },
    fold:true
  },
  computed: {
    totalPrice() {
      let total = 0;
      this.selectFoods.forEach(food => {
        total += food.price * food.count;
      });
      return total;
    },
    totalCount() {
      let count = 0;
      this.selectFoods.forEach(food => {
        count += food.count;
      });
      return count;
    },
    payDesc(){
      if(this.totalPrice===0){
        return `￥${this.minPrice}元起送`;
      }else if(this.totalPrice<this.minPrice){
        let diff=this.minPrice-this.totalPrice
        return `还差￥${diff}元起送`;
      }else{
         return '去结算';
      }
    },
    payClass(){
      if(this.totalPrice<this.minPrice){
        return 'not-enough'
      }else{
         return 'enough'
      }
    },
    listShow(){
      
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.shopcart {
  width: 100%;
  height: 48px;
  position: fixed;
  left: 0;
  bottom: 0;
  z-index: 4;
  background: black;
}
.content {
  display: flex;
  background: #141d27;
  color: rgba(255, 255, 255, 0.4);
}
.content-left {
  flex: 1;
}
.logo-wrapper {
  display: inline-block;
  position: relative;
  top: -10px;
  margin: 0 12px;
  padding: 6px;
  width: 56px;
  height: 56px;
  box-sizing: border-box;
  vertical-align: top;
  border-radius: 50%;
  background: #141d27;
}
.logo {
  width: 100%;
  height: 100%;
  line-height: 46px;
  border-radius: 50%;
  background: #2b343c;
  text-align: center;
}
.hightlight{
  background: rgb(0, 160, 220)
}
.icon-shopping-cart {
  width: 20px;
  height: 20px;
}
.number {
  position: absolute;
  top: 0;
  right: 0;
  width: 24px;
  height: 16px;
  line-height: 16px;
  text-align: center;
  border-radius: 16px;
  font-size: 9px;
  font-weight: 700;
  color: white;
  background: rgb(240, 20, 20);
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.4);
}
.price {
  display: inline-block;
  font-size: 16px;
  font-weight: 700;
  vertical-align: top;
  line-height: 24px;
  padding-right: 12px;
  margin-top: 12px;
  box-sizing: border-box;
  color: rgba(255, 255, 255, 0.4);
  border-right: 1px solid rgba(255, 255, 255, 0.1);
}
.pricehightlight{
  color: #ffffff;
}
.desc {
  display: inline-block;
  vertical-align: top;
  line-height: 24px;
  margin: 12px 0 0 12px;
  font-size: 10px;
  /* color: rgba(255, 255, 255, 0.4); */
}
.content-right {
  flex: 0 0 105px;
  width: 105px;
}
.pay {
  height: 48px;
  line-height: 48px;
  text-align: center;
  font-size: 12px;
  font-weight: 700;
  background: #2b333b;
}
.not-enough{
  background: #2b333b;
}
.enough{
  background: #00b43c;
  color:#ffffff;
}
</style>
