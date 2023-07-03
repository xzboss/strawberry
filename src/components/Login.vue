<template>
    <div class="">
        <div class="login_head">
            <p @click="show=true" :class="show?'display':'hide'">登入</p>
            <p @click="show=false" :class="show?'hide':'display'">新用户？</p>
        </div>
        <div v-if="show" class="login_main">
            <div>
                <label>
                    <input type="email" placeholder="邮件地址" v-model="mail">
                    <span v-if="!RegMail.test(mail)">请输入正确邮箱地址</span>
                    <input type="password" placeholder="密码" v-model="password">
                    <span v-if="!RegPassword.test(password)">密码以字母开头，长度在6~18之间，只能包含字母、数字和下划线</span>
                    <br>
                    <p style="font-size: 10px">忘记密码？</p>
                </label>
                <input type="submit" value="登入" class="button">
            </div>
            <hr>
            <div>
                <b style="font-size: 20px;">访客身份结算</b>
                <label>
                    <input type="email" placeholder="邮箱">
                </label>
                <input type="submit" value="访客登入" class="button">
            </div>
        </div>
        <div v-if="!show" class="login_main">
            <div>
                <label>
                    <input type="text" placeholder="姓名" v-model="name"><span v-if="!RegName.test(name)">请输入合法姓名</span>
                    <input type="text" placeholder="性别" v-model="gender"><span v-if="!RegGender.test(gender)">请输入正确性别</span>
                    <input type="text" placeholder="邮箱" v-model="mail"><span v-if="!RegMail.test(mail)">请输入正确邮箱地址</span>
                    <input type="text" placeholder="密码" v-model="password"><span v-if="!RegName.test(password)">密码以字母开头，长度在6~18之间，只能包含字母、数字和下划线</span>
                    <input type="text" placeholder="再次输入密码">
                </label>
                <input type="button" value="注册" class="button">
            </div>
        </div>
    </div>

</template>
<script>
    const RegMail=/^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/
    const RegPassword=/^[a-zA-Z]\w{5,17}$/
    const RegName=/[\u4e00-\u9fa5]{1-20}/
    const RegGender=/^[男|女]$/
   export default {
      name: "Login",
      data() {
         return {
            show: true,
            mail:'',
            password:'',
            name:'',
            gender:'',
            RegMail:RegMail,
            RegPassword:RegPassword,
            RegName:RegName,
            RegGender:RegGender
         }
      }
   }
</script>
<style scoped>
    /*点击样式*/
    .display{
        border: 2px solid black;

    }
    .hide{
        border: 2px solid transparent;
    }
    /**/
    p{
        cursor: pointer;
    }
    .login_head {
        padding: 20px 0;
        display: flex;
    }
    .login_head > p {
        transition: .3s;
        border-radius: 4px;
        width: 50%;
        font-size: 20px;
        line-height: 50px;
        font-weight: bold;
        color: black;
        text-align: center;
    }
    .login_main > div {
        width: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    label {
        width: 100%;
    }
    input {
        box-sizing: border-box;
        width: 100%;
        outline-style: none;
        border: 1px solid black;
        padding: 6px 12px;
        display: block;
        font-size: 13px;
        line-height: 1.42;
        color: #555;
        background-color: #fff;
        background-image: none;
        margin: 20px 0;
    }
    span{
        margin-top: -15px;
        display: block;
        color: red;
    }
    .button {
        background-color: #623380;
        color: white;
        font-size: 18px;
        font-weight: bold;
        width: 150px;
        border-radius: 3px;
        cursor: pointer;
    }
    hr{
        margin: 50px 0;
    }
</style>