<template>
 <!-- :class="starType"动态绑定星星的尺寸 -->
  <div class="star" :class="starType">
      <!-- 把五个星星循环出来  :class="itemClass" 动态绑定星星状态的class,全星on 半星half 没星off-->
      <span v-for="itemClass in itemClasses" :class="itemClass" class="star-item"></span>
  </div>
</template>

<script>
const length = 5; //星星的长度
const cls_on = "on"; //星星的状态
const cls_half = "half";
const cls_off = "off";

export default {
    // 第一种写法
//   props: {
//     size: {
//       type: Number
//     },
//     score: {
//       type: Number
//     }
//   },
// 第二种写法
  props: ["size", "score"], //通过props接收父组件传递过来的两个参数
  computed: {
    starType() {
      return "star-" + this.size; //返回动态拼接的class
    },
    itemClasses() {
      let result = [];
    //   this.score获取的分数
      let score = Math.floor(this.score * 2) / 2;//向下取0.5倍数的值
      let hasDeximal = score % 1 != 0; //是否有小数（是否有半星）
      let integer = Math.floor(score);//向下取整
      for (let i = 0; i < integer; i++) {//循环有几个全星
        result.push(cls_on);
      }
      if (hasDeximal) { //是否有半星
        result.push(cls_half);
      }
      while (result.length < length) {//补齐没有星
        result.push(cls_off);
      }
      return result;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.star .star-item {
  display: inline-block;
  background-repeat: no-repeat;
}
/* 尺寸为48 */
.star-48 .star-item {
  width: 20px;
  height: 20px;
  margin-right: 22px;
  background-size: 20px 20px;
}
.star-item：last-child {
  margin-right: 0;
}
/* 全星 */
.on {
  background-image: url("./star48_on@2x.png");
}
/* 半星 */
.half {
  background-image: url("./star48_half@2x.png");
}
/* 没有星 */
.off {
  background-image: url("./star48_off@2x.png");
}

/* 尺寸为36 */
.star-36 .star-item {
  width: 15px;
  height: 15px;
  margin-right: 6px;
  background-size: 15px 15px;
}
.star-item：last-child {
  margin-right: 0;
}
/* 全星 */
.on {
  background-image: url("star36_on@2x.png");
}
/* 半星 */
.half {
  background-image: url("star36_half@2x.png");
}
/* 没有星 */
.off {
  background-image: url("star36_off@2x.png");
}
/* 尺寸为24 */
.star-24 .star-item {
  width: 10px;
  height: 10px;
  margin-right: 3px;
  background-size: 10px 10px;
}
.star-item：last-child {
  margin-right: 0;
}
/* 全星 */
.on {
  background-image: url("star24_on@2x.png");
}
/* 半星 */
.half {
  background-image: url("star24_half@2x.png");
}
/* 没有星 */
.off {
  background-image: url("star24_off@2x.png");
}
</style>
