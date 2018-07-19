<template>
  <div class="header">
    <div class="content-wrapper">
        <div class="avatar">
          <img width="64" height="64" :src="seller.avatar">
        </div>
        <div class="content">
          <div class="title">
            <span class="brand"></span>
            <span class="name">{{seller.name}}</span>
          </div>
          <div class="description">
            {{seller.description}}/ {{seller.deliveryTime}}分钟送达
          </div>
          <div v-if="seller.supports" class="support">
            <span class="icon" :class="mapClass[seller.supports[0].type]"></span>
            <span class="text">{{seller.supports[0].description}}</span>
          </div>

        </div>
        <div class="support-count" v-if="seller.supports" @click="showDetail">
          <span class="count">{{seller.supports.length}}个</span>
          <i>></i>
        </div>
    </div>
    <div class="bulletin-wrapper" @click="showDetail">
        <span class="bulletin-title"></span>
         <span class="bulletin-text">{{seller.bulletin}}</span>
    </div>
    <div class="background">
      <img :src="seller.avatar" width="100%" height="100%">
    </div>
    <!-- 弹层部分 -->
    <transition name="fade">
    <div class="detail" v-show="detailShow">
      <div class="detail-wrapper clearfix">
        <div class="detail-main">
          <h1 class="name">{{seller.name}}</h1>
          <div class="star-wrapper">
            <star :size='48' :score='seller.score'></star> 
          </div> 
          <div class="title">
            <div class="line"></div>
            <div class="text">优惠信息</div>
            <div class="line"></div>
          </div> 
          <ul v-if="seller.supports" class="supports">
            <li class="support-item" v-for="(item,index) in seller.supports">
              <span class="icon" :class="mapClass[seller.supports[index].type]"></span>
              <span class="text">{{seller.supports[index].description}}</span>
            </li>
          </ul>
           <div class="title">
            <div class="line"></div>
            <div class="text">商家公告</div>
            <div class="line"></div>
          </div> 
          <div class="bulletin">
            <p class="content">{{seller.bulletin}}</p>
          </div>
        </div>
      </div>
      <div class="detail-close" @click="close">
        <span class="icon-close">X</span>
      </div>
    </div>
    </transition> 
  </div>
</template>

<script>
import axios from "axios";
import star from "../star/star";
export default {
  data() {
    return {
      seller: {},
      mapClass: [],
      detailShow: false
    };
  },
  components: {
    star
  },
  methods: {
    showDetail() {
      this.detailShow = true;
    },
    close(){
      this.detailShow=false;
    }
  },
  created() {
    axios
      .get("https://wujinging.github.io/elema/api/seller/")
      .then(response => {
        if (response.data.errno === 0) {
          this.seller = response.data.data;
        }
      })
      .catch(error => {
        console.log(error);
      });
    this.mapClass = ["decrease", "discount", "special", "invoice", "guarantee"];
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.header {
  background: rgba(7, 17, 27, 0.5);
  color: #ffffff;
  position: relative;
}
.content-wrapper {
  padding: 24px 12px 18px 24px;
  font-size: 0px;
  position: relative;
}
.avatar,
.content {
  display: inline-block;
  font-size: 14px;
}
.avatar {
  vertical-align: top;
}
.avatar img {
  border-radius: 2px;
}
.content {
  margin-left: 16px;
}
.title {
  margin: 2px 0 8px 0;
}
.brand {
  width: 30px;
  height: 18px;
  display: inline-block;
  background-image: url("../../../resource/img/brand@2x.png");
  background-size: 30px 18px;
  vertical-align: top;
}
.title .name {
  margin-left: 6px;
  font-size: 16px;
  line-height: 18px;
  font-weight: bold;
}
.description {
  margin-bottom: 5px;
  line-height: 12px;
  font-size: 12px;
}
.icon {
  display: inline-block;
  width: 12px;
  height: 12px;
  margin-right: 2px;
  background-size: 12px 12px;
  background-repeat: no-repeat;
  vertical-align: middle;
}
.decrease {
  background-image: url("../../../resource/img/decrease_1@2x.png");
}
.discount {
  background-image: url("../../../resource/img/discount_1@2x.png");
}
.guarantee {
  background-image: url("../../../resource/img/guarantee_1@2x.png");
}
.invoice {
  background-image: url("../../../resource/img/invoice_1@2x.png");
}
.special {
  background-image: url("../../../resource/img/special_1@2x.png");
}
.text {
  font-size: 10px;
  line-height: 12px;
}
.support-count {
  position: absolute;
  right: 12px;
  bottom: 18px;
  padding: 0 8px;
  height: 24px;
  line-height: 24px;
  border-radius: 14px;
  background: rgba(0, 0, 0, 0.2);
  text-align: center;
  font-size: 10px;
}
.bulletin-wrapper {
  position: relative;
  height: 28px;
  line-height: 28px;
  padding: 0 22ox 0 12px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  background: rgba(7, 17, 27, 0.2);
}
.bulletin-title {
  display: inline-block;
  width: 22px;
  height: 12px;
  margin-left: 10px;
  background-image: url("../../../resource/img/bulletin@2x.png");
  background-size: 22px 12px;
  vertical-align: middle;
}
.bulletin-text {
  font-size: 10px;
  margin: 0 5px 0 2px;
}
.background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
  filter: blur(10px);
}
/* 弹层部分 */
.detail {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(7, 17, 27, 0.8);
  -webkit-transition: all 0.5s;
  -moz-transition: all 0.5s;
  transition: all 0.5s;
  z-index: 100;
}
.fade-transition{
  opacity: 1; 
  background: rgba(7, 17, 27, 0.8)
}
.fade-enter,.fade-leave{
  opacity: 0;
  background: rgba(7, 17, 27, 0)
}
.clearfix {
  display: inline-block;
}
.clearfix:after {
  display: block;
  content: ".";
  height: 0;
  line-height: 0;
  clear: both;
  visibility: hidden;
}

.detail-wrapper {
  min-height: 100%;
  width: 100%;
}
.detail-main {
  margin-top: 64px;
  padding-bottom: 64px;
}
.detail-main .name {
  line-height: 16px;
  text-align: center;
  font-size: 16px;
  font-weight: 700;
}
.star-wrapper{
  text-align: center;
  margin-top: 18px;
  padding: 2px 0;
}
.detail .title{
  display: flex;
  width: 80%;
  margin: 28px auto 24px auto;
}
.line{
  flex: 1;
  position: relative;
  top: -6px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.2);
}
.detail .text{
  padding: 0 12px;
  font-size: 14px;
  font-weight: 700;
}
.detail .supports{
  width: 80%;
  margin: 0 auto;
}
.detail .support-item{
  padding: 0 12px;
  margin-bottom: 12px;
  font-size: 0;
}
.detail .icon{
  display: inline-block;
  width: 16px;
  height: 16px;
  vertical-align: top;
  margin-right: 6px;
  background-size: 16px 16px;

}
.detail .supports .text{
  line-height: 16px;
  font-size: 12px;
  font-weight: 100;
}
.detail .bulletin{
  width: 80%;
  margin: 0 auto;
}
.detail .content{
  padding: 0 12px;
  line-height: 24px;
  font-size: 12px;
}
.detail .support-item:last-child{
  margin-bottom: 0;
}
.detail-close {
  position: relative;
  width: 32px;
  height: 32px;
  margin: -64px auto 0 auto;
  clear: both;
  font-size: 16px;
}
</style>
