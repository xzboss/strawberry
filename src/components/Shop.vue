<template>
    <div class="box">
        <div class="head">购 物 车</div>
        <div class="main">
            <div class="list">
                <div class="list_top">
                    <p>您现可享免费标准付运</p>
                    <p></p>
                    <p>祝您购物愉快</p>
                    <p>草莓网出库商品</p>
                </div>
                <div class="list_mid">
                    <ul>
                        <li v-for="(item,index) in commodity" :key="index">
                            <div class="list_mid_left">
                                <img :src="item.address" alt="">
                                <div>
                                    <p>{{item.name}}</p>
                                    <p style="width: 200px">{{item.describe}}</p>
                                    <p><span style="font-size: 10px">CNY ￥ </span>{{item.price}}</p>
                                </div>
                            </div>
                            <div class="list_mid_mid">
                                <span @click="reduce(item.level,index)"></span>数量 {{quantity[index].number}}
                                <span @click="add(item.level,index)"></span>
                            </div>
                            <div class="list_mid_right">
                                <span style="font-size: 10px">CNY ￥ </span>{{(item.price*quantity[index].number).toLocaleString()+'.00'}}
                                <i @click="remove(item.level,index)"></i>
                            </div>
                        </li>
                        <li class="discount">
                            <div class="discount_left">
                                <b>订单总额： {{num}} 物品</b>
                                <p>Extra 15% Off (Women's Day Sale CN)</p>
                                <p>标准运付</p>
                            </div>
                            <div class="discount_right">
                                <b><span>CNY ￥ </span>{{sum.toLocaleString()+'.00'}}</b>
                                <p><span> - CNY ￥ </span>{{discountPrice.toLocaleString()}}</p>
                                <p><span>CNY ￥ </span>0.00</p>
                            </div>
                        </li>
                        <li id="sum">
                            <p>
                                订单总额： {{num}} 物品
                                <br>
                                <span>(含税)</span>
                            </p>
                            <p>
                                <span style="font-weight: bold;">CNY ¥ </span>
                                {{pay.toLocaleString()}}
                            </p>
                        </li>
                    </ul>
                </div>
            </div>
            <div class="login">
                <Login></Login>
            </div>
        </div>
    </div>
</template>
<script>
   import Login from "./Login";
   import axios from 'axios'
   export default {
      name: "Shop",
      components: {
         Login: Login/*自定义标签*/
      },
      data() {
         return {
            products: [],/*所有商品信息，包括没选的*/
            commodity: [],/*渲染商品列表*/
            quantity: [],/*商品数量*/
            discount: 0.75,/*折扣*/
         }
      },
      watch: {/*子组件向父组件传递参数*/
         num(newValue) {
            this.$emit('change', newValue)
         }
      },
      computed: {
         /*计算商品数量*/
         num() {
            let num = 0
            this.quantity.forEach(item => {
               num += item.number
            })
            return num
         }
         /*计算付款总额度*/
         , sum() {
            let sum = 0
            this.commodity.forEach((item, index) => {
               sum += item.price * this.quantity[index].number/*对应属性相加*/
            })
            return sum
         },
         /*计算折扣价钱*/
         discountPrice() {
            return this.sum * (1 - this.discount)
         },
         /*计算付款总额*/
         pay() {
            return this.sum - this.discountPrice
         }
      },
      mounted() {
         axios.get('data/json/products.json').then(res => {
            this.products = res.data/*将商品全部赋值给参数product*/
            /*再通过循环遍历将对应本地存储中有的商品编号对应的商品赋值给需要渲染的参数*/
            for (let value of this.products) {
               for (let i = 1, judgment; i <= this.products.length; i++) {
                  if (JSON.parse(localStorage.getItem('' + i)) !== null) {/*需要当前标号商品存在本地存储再判断哪些商品该渲染*/
                     judgment = JSON.parse(localStorage.getItem('' + i)).level
                     /*通过判断本地存储中*/
                     if (judgment === value.level) {
                        this.commodity.push(value)/*在商品后面添加其他商品*/
                        this.quantity.push(JSON.parse(localStorage.getItem('' + i)))/*各商品数量信息添加到quantity数组中*/
                     }
                  }
               }
            }
         })
      },
      methods: {
         /*删除商品*/
         remove(level, index) {
            this.quantity.splice(index, 1)/*删除对应商品数量*/
            this.commodity.splice(index, 1)/*删除对应商品信息*/
            localStorage.removeItem(level)/*删除对应本地存储商品信息数量*/
         },
         /*减少商品数量*/
         reduce(level, index) {/*传入对应商品编号和下标*/
            if (this.quantity[index].number) {/*防止减到负数*/
               this.quantity[index].number--
               localStorage.setItem(level, JSON.stringify({"level": level, "number": this.quantity[index].number}))
            }
         },
         /*增加商品数量*/
         add(level, index) {
            this.quantity[index].number++
            localStorage.setItem(level, JSON.stringify({"level": level, "number": this.quantity[index].number}))
         },
         shiyan() {
            console.log(typeof JSON.parse(localStorage.getItem(1)).number)
         }
      }
   }
</script>
<style scoped>
    /*登录*/
    .login {
        width: 330px;
        height: 640px;
        background-color: white;
        padding: 30px;
        border-radius: 3px;
    }
    .box {
        padding: 0 12%;
        background-color: #F5F5F5;
    }
    /*标题*/
    .head {
        padding: 50px 0 30px 0;
        font-size: 32px;
        font-weight: bold;
    }
    .main {
        display: flex;
        justify-content: space-between;
    }
    /*商品*/
    .list {
        background-color: white;
        border-radius: 2px;
        width: 65%;
    }
    .list_top {
        padding: 20px;
        font-size: 14px;
    }
    .list_top p {
        margin-top: 10px;
        opacity: .9;
    }
    .list_top p:nth-of-type(2) {
        width: 100%;
        height: 10px;
        border-radius: 5px;
        background-color: #d497f0;
    }
    /*商品栏*/
    .list_mid {
        padding: 0 20px;
    }
    .list_mid li {
        padding: 10px 30px 10px 10px;
        border-top: 1px solid rgba(0, 0, 0, .2);
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .list_mid_left {
        display: flex;
        align-items: start;
    }
    .list_mid_left img {
        width: 100px;
    }
    .list_mid_left > div {
        font-size: 14px;
        display: flex;
        justify-content: space-between;
        flex-direction: column;
    }
    .list_mid_left > div p {
        line-height: 25px;
    }
    .list_mid_left > div p:nth-of-type(1) {
        font-weight: bold;
    }
    .list_mid_mid {
        width: 112px;
        display: flex;
        align-items: center;
        justify-content: space-between;
    }
    select {
        border: transparent;
        outline-style: none;
    }
    .list_mid_mid span {
        margin: 0 4px;
        width: 20px;
        height: 20px;
        display: inline-block;
    }
    .list_mid_mid span:nth-of-type(1) {
        background: url("../assets/imgs/subtract.png") no-repeat;
        background-size: cover;
    }
    .list_mid_mid span:nth-of-type(2) {
        background: url("../assets/imgs/add.png") no-repeat;
        background-size: cover;
    }
    .list_mid_right {
        width: 125px;
        font-weight: bold;
        position: relative;
    }
    .list_mid_right i {
        cursor: pointer;
        right: -20px;
        top: -5px;
        position: absolute;
        opacity: .7;
        display: inline-block;
        width: 10px;
        height: 10px;
        background: url("../assets/imgs/fork.png") no-repeat;
        background-size: cover;
    }
    /*折扣区*/
    .discount {
        line-height: 30px;
    }
    .discount > div > p:first-of-type {
        color: #e82e74;
    }
    .discount_left {
        font-size: 14px;
    }
    .discount_right {
        display: flex;
        flex-direction: column;
        align-items: end;
        font-size: 16px;
    }
    .discount_right span {
        font-size: 10px;
    }
    /*总价区*/
    #sum {
        padding-top: 20px;
        display: flex;
        align-items: start;
        border-top: 1px solid black;
        line-height: 30px;
        font-size: 18px;
        font-weight: bold;
    }
    #sum span {
        font-weight: normal;
        font-size: 10px;
    }
</style>