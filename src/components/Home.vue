<template>
    <div class="box">
        <div class="wheel">
            <div>
                <img :src=address alt="">
            </div>
            <div>
                <ul class="other1">
                    <li v-for="(item,index) in imgs" :key="index" @click="change(item,'')"></li>
                </ul>
            </div>
        </div>
        <div class="head">
            <hr>
            <b>女 神 , 你 说 了 算 ! </b>
            <hr>
        </div>
        <div class="product">
            <ul>
                <li v-for="(value,key) in products" :key="key">
                    <div class="product_box">
                        <img :src=value.address alt="">
                    </div>
                    <b>{{value.name}}</b>
                    <p>{{value.describe}}</p>
                    <span>￥{{value.price}}</span><br>
                    建议零售价
                    <del>￥{{value.oldPrice}}</del>
                    <br>
                    (包税)
                    <br>
                    <button @click="add(value.level)">加入购物车</button>
                </li>
            </ul>
        </div>
    </div>
</template>
<script>
   import axios from 'axios'
   export default {
      name: "Home",
      data() {
         return {
            address: "../data/imgs/wheel1.jpg",
            imgs: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15],
            products: [],
            arr: [],
            timer: null,
            step: 1,
            new:document.hidden
         }
      },
      methods: {
         change(item,step) {
            let mid=item
            if(item===''){
               mid=step
            }
            let liArr = document.querySelectorAll('.wheel li')
            liArr.forEach(item => item.style.background = 'white')/*初始化所有小圈颜色*/
            liArr.forEach(() => liArr[mid - 1].style.background = '#521b76')/*改变相应小圈背景颜色*/
            this.address = "../data/imgs/wheel" + mid + ".jpg"
            this.step=mid+1
            if(mid===15){
               this.step=1
            }
         },
         add(level) {
            /*如果本地存储不存在此商品，再执行*/
            if (!localStorage.getItem(level)) {
               this.$emit('addition')/*向父组件传事件，方便调用父组件方法*/
               localStorage.setItem(level, JSON.stringify({level: level, number: 1}))/*往本地存储存储相应商品标号及数量的json对象*/
            }
            this.move()
         },
         move() {
            console.log(48948949)
         }
      },
      mounted() {/*在模板渲染成html后调用，通常是初始化页面完成后，再对html的dom节点进行一些需要的操作*/
         /*axios工具获取jsons文件中的数据*/
         axios.get('data/json/products.json').then(res => {
            this.products = res.data
         })
         this.timer=setInterval(()=>{
            this.change('',this.step++)
         }, 5000)
         this.$once('hook:beforeDestory',()=>{
            clearInterval(this.timer)
         })
      },
      destroyed() {/*离开页面清除定时器*/
         if(this.timer){
            clearInterval(this.timer)
         }
      }
   }
</script>
<style scoped>
    .box {
        padding: 0 12%;
    }
    /*轮播图----------------------------------------------------------------*/
    .other1 li:first-of-type{
        background-color: #521b76;
    }
    .wheel {
        padding-top: 40px;
    }
    .wheel img {
        width: 100%;
        height: 308px;
        border: 1px solid rgba(0, 0, 0, .1);
    }
    .wheel ul {
        margin: 30px 0;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .wheel li {
        box-sizing: border-box;
        margin: 0 10px;
        width: 12px;
        height: 12px;
        border-radius: 50%;
        border: 2px solid #521b76;
        cursor: pointer;
    }
    /*标题---------------------------------------------------------------------------*/
    .head {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 30px;
    }
    .head > hr {
        height: 5px;
        background-color: #521b76;
        width: 40%;
        display: inline-block;
    }
    .head > b {
        font-size: 19px;
        display: inline-block;
    }
    /*商品-------------------------------------------------------------------------*/
    .product {
        font-size: 12px;
        opacity: .8;
    }
    .product ul {
        padding: 0 10px;
        overflow: hidden; /*给浮动元素父元素设置此值以解决元素坍塌问题*/
        display: flex;
        justify-content: start;
        flex-wrap: wrap;
    }
    .product li {
        margin: 6px;
        border-radius: 5px;
        width: 230px;
        padding: 20px;
    }
    .product li:hover {
        box-shadow: 2px 3px 10px rgba(0, 0, 0, .3);
    }
    .product div {
        width: 230px;
        height: 230px;
        background: url("../../public/data/imgs/productbox.png") no-repeat;
        background-size: cover;
    }
    .product img {
        transform: translate(15px, 15px); /*在以自身中心为原点上下左右平移*/
        width: 200px;
    }
    .product b {
        display: block;
        margin-top: 10px;
        line-height: 30px;
        font-size: 14px;
        opacity: 1;
    }
    .product p {
        height: 40px;
        line-height: 20px;
        font-size: 12px;
        margin-bottom: 30px;
    }
    .product span {
        opacity: 1;
        font-size: 20px;
        line-height: 30px;
        font-weight: bold;
        color: #ee2c72;
    }
    .product del {
        line-height: 20px;
    }
    .product button {
        cursor: pointer;
        border: transparent;
        width: 100px;
        height: 33px;
        opacity: 1;
        font-size: 16px;
        border-radius: 5px;
        background-color: #00a99c;
        color: white;
        font-weight: bold;
        transform: translate(75px);
        transition: .2s;
    }
    .product button:hover {
        background-color: #2980b9;
    }
</style>