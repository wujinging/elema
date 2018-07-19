<template>
<div class="goods">
  <div class="menu-wrapper" ref="menuwrapper">
   <ul>
     <li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex===index}" @click="selectMenu(index,$event)">
       <span class="text">
         <span v-show="item.type>0" class="icon" :class="mapClass"></span>{{item.name}}
       </span>
     </li>
   </ul>
  </div>
  <div class="foods-wrapper" ref="foodWrapper">
    <ul>
      <li v-for="item in goods" class="foods-list food-list-hook">
        <h1 class="title">{{item.name}}</h1>
        <ul>
          <li v-for="food in item.foods" class="food-item">
            <div class="icon">
              <img width="57" height="57" :src="food.icon">
            </div>
            <div class="content">
              <p class="name">{{food.name}}</p>
              <p class="desc">{{food.description}}</p>
              <div class="extra">
                <span class="count">月售{{food.sellCount}}份</span>
                <span>好评率{{food.rating}}%</span>
              </div>
              <div class="price">
                <span class="now">￥{{food.price}}</span>
                <span v-show="food.oldPrice" class="old">￥{{food.oldPrice}}</span>
              </div>
              <div class="cartcontrol-wrapper">
                <cartcontrol :food="food"></cartcontrol> 
              </div>
            
            </div>
          </li>
        </ul>
      </li>
    </ul>
  </div>
  <shopcart :selectFoods="selectFoods" :delivery-price="seller1.deliveryPrice" :min-price="seller1.minPrice"></shopcart>  
</div>
  
</template>

<script>
import axios from "axios";
import BScroll from "better-scroll";
import shopcart from "../../components/shopcart/shopcart";
import cartcontrol from "../../components/cartcontrol/cartcontrol";
export default {
  props: {
    seller: {
      type: Object
    }
  },
  data() {
    return {
      seller1: {},
      goods: [],
      listHeight: [],
      scrollY: 0
    };
  },
  components: { 
    shopcart,
    cartcontrol
 },
  computed: {
    currentIndex() {
      for (let i = 0; i < this.listHeight.length; i++) {
        //判断当currentIndex在height1和height2之间的时候显示
        let height1 = this.listHeight[i];
        let height2 = this.listHeight[i + 1];
        //最后一个区间没有height2
        if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
          return i;
        }
      }
      return 0;
    },
    selectFoods(){
      let foods=[];
      this.goods.forEach(good=>{
        good.foods.forEach(food=>{
          if(food.count){
            foods.push(food)
          }
        })
      })
      return foods;
    }
  },
  created() {
    this.mapClass = ["decrease", "discount", "special", "invoice", "guarantee"];
    axios
      .get("/api/goods")
      .then(response => {
        if (response.data.errno === 0) {
          this.goods = response.data.data;
          //dom结构加载结束
          this.$nextTick(() => {
            this._initScroll();
            this._calculateHeight();
          });
        }
      })
      .catch(error => {
        console.log(error);
      });
    axios
      .get("/api/seller")
      .then(response => {
        if (response.data.errno === 0) {
          this.seller1 = response.data.data;
        }
      })
      .catch(error => {
        console.log(error);
      });
  },
  methods: {
    selectMenu(index, event) {
      //      自己默认派发事件时候(BScroll),_constructed被置为true,但是浏览器原生并没有这个属性
      if (!event._constructed) {
        return;
      }
      //运用BScroll接口，滚动到相应位置
      let foodList = this.$refs.foodWrapper.getElementsByClassName(
        "food-list-hook"
      );
      //获取对应元素的列表
      let el = foodList[index];
      //设置滚动时间
      this.foodScroll.scrollToElement(el, 300);
    },
    _initScroll() {
      this.menuScroll = new BScroll(this.$refs.menuwrapper, {
        click: true
      });
      this.foodScroll = new BScroll(this.$refs.foodWrapper, {
        click: true,
        //探针作用，实时监测滚动位置
        probeType: 3
      });
      //设置监听滚动位置
      this.foodScroll.on("scroll", pos => {
        //scrollY接收变量
        this.scrollY = Math.abs(Math.round(pos.y));
      });
    },
    _calculateHeight() {
      // 找到li元素
      let foodList = this.$refs.foodWrapper.getElementsByClassName(
        "food-list-hook"
      );
      let height = 0;
      //把第一个高度送入数组
      this.listHeight.push(height);
      //通过循环foodList下的dom结构，将每一个li的高度依次送入数组
      for (let i = 0; i < foodList.length; i++) {
        let item = foodList[i];
        height += item.clientHeight;
        this.listHeight.push(height);
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
li {
  list-style: none;
}
.goods {
  display: flex;
  position: absolute;
  top: 179px;
  bottom: 46px;
  width: 100%;
  overflow: hidden;
}
.goods .menu-wrapper {
  flex: 0 0 80px;
  width: 80px;
  background: #f3f5f7;
}
.menu-item {
  display: table;
  width: 56px;
  height: 54px;
  line-height: 14px;
  padding: 0 12px;
}
.current {
  position: relative;
  margin-top: -1px;
  z-index: 10;
  background: #ffffff;
  font-weight: 700;
}
.current .text {
  border: none;
}
.menu-wrapper .icon {
  display: inline-block;
  width: 12px;
  height: 12px;
  margin-right: 2px;
  background-size: 12px 12px;
  background-repeat: no-repeat;
  vertical-align: middle;
}
.decrease {
  background-image: url("../../../resource/img/decrease_3@2x.png");
}
.discount {
  background-image: url("../../../resource/img/discount_3@2x.png");
}
.guarantee {
  background-image: url("../../../resource/img/guarantee_3@2x.png");
}
.invoice {
  background-image: url("../../../resource/img/invoice_3@2x.png");
}
.special {
  background-image: url("../../../resource/img/special_3@2x.png");
}
.text {
  font-size: 12px;
  display: table-cell;
  width: 54px;
  vertical-align: middle;
  border-bottom: 1px solid rgba(7, 17, 27, 0.1);
}
.foods-wrapper {
  flex: 1;
}
.title {
  padding-left: 14px;
  height: 26px;
  line-height: 26px;
  border-left: 2px solid #d9dde1;
  font-size: 12px;
  color: rgb(147, 153, 159);
  background: #f3f5f7;
}
.food-item {
  display: flex;
  margin: 18px;
  padding-bottom: 18px;
  border-bottom: 1px solid rgba(7, 17, 27, 0.1);
}
.food-item:last-child {
  border-bottom: none;
  margin-bottom: 0;
}
.foods-wrapper .icon {
  flex: 0 0 57px;
  margin-right: 10px;
}
.foods-wrapper .content {
  position: relative;
  flex: 1;
}
.foods-wrapper .name {
  margin: 2px 0 8px 0;
  height: 14px;
  line-height: 14px;
  font-size: 14px;
  color: rgb(7, 17, 27);
}
.desc,
.extra {
  margin-bottom: 8px;
  line-height: 10px;
  font-size: 10px;
  color: rgb(147, 153, 159);
}
.desc {
  margin-bottom: 8px;
  line-height: 14px;
}
.extra .count {
  margin-right: 12px;
}
.price {
  font-weight: 700;
  line-height: 24px;
}
.now {
  margin-right: 8px;
  font-size: 14px;
  color: rgb(240, 20, 20);
}
.old {
  text-decoration: line-through;
  font-size: 10px;
  color: rgb(147, 153, 159);
}
.cartcontrol-wrapper{
  position: absolute;
  right: 0;
  bottom: -6px;
}
</style>
