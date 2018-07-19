<template>
  <div>
      <v-header></v-header>
      <div class="tap">
        <router-link to="/goods" class="tap-item">商品</router-link>
        <router-link to="/ratings" class="tap-item">评价</router-link>
        <router-link to="/sellers" class="tap-item">商家</router-link>
      </div>
      <router-view></router-view>
  </div>
</template>

<script>
import header from "./components/header/header";
import axios from 'axios'
export default {
  
  components: {
    "v-header": header
  }
};
</script>

<style>
*{
  margin: 0;padding: 0;
}
 .tap{
  display: flex;
  height: 40px;
  line-height: 40px;
  border-bottom: 1px solid rgba(7, 17, 27, 0.1);
}
.tap-item{
   flex: 1;
   text-align: center;
}
a{
  color:rgb(77, 85, 93);
  text-decoration: none;
  font-size: 16px;
}
.router-link-active{
   color:rgb(240, 20, 20);
}
</style>
