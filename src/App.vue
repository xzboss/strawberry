<template>
  <div id="app">
    <div class="head">
      <div class="head_top">
        <router-link to="/Home" class="head_top_left"></router-link>
        <div class="head_top_mid">
          <div>
            <input type="text" placeholder="搜索商品/品牌/关键词">
            <span></span>
          </div>
          <ul>
            <li><a href="">Natural Beauty</a></li>
            <li><a href="">Estee Lauder</a></li>
            <li><a href="">Shiseido</a></li>
            <li><a href="">Double Serum</a></li>
          </ul>
        </div>
        <div class="head_top_right">
          <ul>
            <li>
              ¥ CNY China: 简体
              <span>
                <b>v</b>
              </span>
            </li>
            <li>订单查询</li>
          </ul>
          <ul>
            <li>
              <span class="span1"></span>
              <b>登入</b>
            </li>
            <li>
              <span class="span2"></span>
              <b>心愿单</b>
            </li>
            <li>
              <span class="span3"></span>
              <b>
                <router-link to="/Shop">购物篮</router-link>
              </b>
              <div class="shopNumber">{{num}}</div>
            </li>
          </ul>
        </div>
      </div>
      <div class="head_bottom">
        <ul>
          <li>根据品牌选购</li>
          <li>女士护肤</li>
          <li>品牌彩妆</li>
          <li>美发护发</li>
          <li>女士香水</li>
          <li>男士护肤</li>
          <li>男士香水</li>
          <li>健康食品</li>
          <li>NB 東森自然美</li>
          <li>促销活动</li>
          <li>新品上线</li>
        </ul>
      </div>
    </div>
    <div class="main">
      <router-view @change="change" @addition="addition"></router-view>
    </div>
    <div class="footer">
      <div class="footer_top">
        <div class="footer_top_left">
          <p>关注我们</p>
          <div>
            <input type="text" placeholder="输入你的电邮以获取最新优惠资讯">
            <span>注册</span>
          </div>
        </div>
        <div class="footer_top_mid">
          <span></span>
          <div>
            来自<br>
            <b>800</b><br>
            多个品牌<br>
            <b>33,000</b>件产品<br>
          </div>
        </div>
        <div class="footer_top_right">
          <span></span>
          <div>
            <p>免费付运至全球</p>
            <span>适用条件</span>
          </div>
        </div>
      </div>
      <div class="footer_mid"></div>
      <div class="footer_bottom"></div>
    </div>
  </div>
</template>
<script>
import VueRouter from 'vue-router'/*引入路由器*/
import Vue from 'vue'/*引入vue实例*/
import Home from "./components/Home";
import Shop from "./components/Shop"
Vue.use(VueRouter)
let routes = [
  { path: '/home', component: Home },
  { path: '/Shop', component: Shop },
]
let router = new VueRouter({ routes })
export default {
  name: 'App',
  router: router,
  data () {
    return {
      num: 0
    }
  },
  methods: {
    /*购物车数量改变时执行函数*/
    change (newValue) {
      this.num = newValue
    },
    /*Home点击添加到购物车执行函数*/
    addition () {
      this.num++
    }
  },
  mounted () {
    this.$router.push ('/home')
    /*初始加载物品数量*/
    for (let i = 0; i < 100; i++) {
      if (localStorage.getItem(i + '')) {
        this.num += JSON.parse(localStorage.getItem(i + '')).number
      }
    }
  }
}
</script>
<style scoped>
.shopNumber {
  padding: 0 4px;
  border-radius: 50%;
  background-color: #fe7202;
  position: absolute;
  top: -4px;
  left: 21px;
  color: white;
  text-align: center;
  line-height: 15px;
}
#app {
  width: 100%;
}
.head {
  width: 100%;
  height: 150px;
}
/*log-搜索-按钮----------------------------------------------------------------------------------*/
.head_top {
  padding: 0 12%;
  height: 110px;
  background: url('./assets/imgs/strawberry.jpg') no-repeat;
  background-size: cover;
  background-position-x: -350px; /*调整背景左右位移量*/
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.head_top_left {
  display: block;
  width: 268px;
  height: 63px;
  background: url('./assets/imgs/log.png') no-repeat;
}
/*搜索栏----------------------------------------------------------------------------------*/
.head_top_mid {
  width: 420px;
}
.head_top_mid > div {
  border-radius: 5px;
  border: 2px solid #521b76;
  box-sizing: border-box; /*改变总高度计算方法，防止溢出 在 CSS 盒子模型 元素所的 width 与 height 会应用到元素的内外边距*/
  padding: 8px 10px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.head_top_mid input {
  width: 80%;
  line-height: 14px;
  outline: none; /*取消获取焦点效果*/
  border: transparent; /*隐藏边框*/
}
input::placeholder {
  color: #521b76; /*改变提示词字体颜色*/
}
.head_top_mid > div > span {
  width: 20px;
  height: 20px;
  background-size: cover;
  background-image: url('./assets/imgs/search.png');
}
.head_top_mid > ul {
  margin-top: 10px;
  color: rgba(0, 0, 0, 0.5);
  font-size: 14px;
  display: flex;
  justify-content: space-between;
}
.head_top_mid li:hover {
  /*伪类选择器*/
  color: #521b76;
}
/*按钮栏----------------------------------------------------------------------------------*/
.head_top_right {
  width: 250px;
  font-size: 12px;
  color: #b53788;
  font-weight: bold;
}
.head_top_right ul:nth-of-type(1) {
  justify-content: right;
  padding-bottom: 10px;
}
.head_top_right ul {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.head_top_right ul li {
  position: relative;
  line-height: 33px;
  display: flex;
  align-items: center;
}
.head_top_right span {
  width: 33px;
  height: 33px;
  display: inline-block;
}
.head_top_right b {
  margin-left: 5px;
}
/*登入-心愿单-购物车精灵图放置*/
.span1 {
  background: url('./assets/imgs/sprite.png') -277px -315px no-repeat;
}
.span2 {
  background: url('./assets/imgs/sprite.png') -349px -315px no-repeat;
}
.span3 {
  background: url('./assets/imgs/sprite.png') -313px -315px no-repeat;
}
/*导航条----------------------------------------------------------------------------------*/
.head_bottom {
  padding: 0 12%;
  background-color: #623381;
}
.head_bottom ul {
  display: flex;
  justify-content: space-between;
}
.head_bottom ul li {
  cursor: pointer;
  padding: 0 10px;
  line-height: 40px;
  color: white;
  font-size: 12px;
  font-weight: bold;
  transition: 0.2s; /*改变变化时间*/
}
.head_bottom li:hover {
  color: #623381;
  background-color: white;
}
/*底部说明----------------------------------------------------------------------------------*/
.footer {
  width: 100%;
}
.footer_top {
  padding: 50px 12%;
  background: url('./assets/imgs/footer_top_bg.jpg') no-repeat;
  background-size: cover;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.footer_top_left p {
  font-size: 24px;
  color: white;
  margin-bottom: 30px;
}
.footer_top_left > div {
  display: flex;
  align-items: center;
}
.footer_top_left > div input {
  width: 300px;
  outline-style: none;
  border: none;
  height: 50px;
  padding: 5px 10px;
  box-sizing: border-box;
  margin-right: 1px;
}
.footer_top_left > div span {
  width: 50px;
  display: inline-block;
  background-color: #521b76;
  color: white;
  font-size: 12px;
  text-align: center;
  line-height: 50px;
  border-radius: 0 3px 3px 0;
}
.footer_top_left > div span:hover {
  background-color: white;
  color: #521b76;
}
.footer_top_mid {
  display: flex;
  align-items: center;
}
.footer_top_mid > span {
  display: inline-block;
  width: 75px;
  height: 95px;
  background: url('assets/imgs/sprite.png') -89px -381px no-repeat;
}
.footer_top_mid > div {
  padding-left: 30px;
  color: white;
  display: inline-block;
  font-size: 18px;
}
.footer_top_mid > div > b {
  font-size: 20px;
}
.footer_top_right {
  display: flex;
  align-items: center;
}
.footer_top_right > span {
  width: 84px;
  height: 91px;
  background: url(./assets/imgs/freepay.png) -9px -5px no-repeat;
}
.footer_top_right > div {
  margin-left: 30px;
}
.footer_top_right > div > p {
  color: white;
  font-size: 28px;
  font-weight: bold;
  margin-bottom: 40px;
}
.footer_top_right > div > span {
  color: white;
}
</style>
