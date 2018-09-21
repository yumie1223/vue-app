<template>
<div class="userForm">
  <header>
    <a class= "back" @click= 'back'><em></em></a>
    <span>注册</span>
  </header>
  <div class="register">
    <div class="form-wrapper register-wrapper">
      <mt-field placeholder="请输入手机号码" v-model="tel" type="tel" :state="telState"></mt-field>
      <mt-field placeholder="请输入验证码" v-model='code'>
        <span @click='sendCode'>发送验证码</span>
      </mt-field>
      <mt-field placeholder="请输入密码" :state='passwordState' type="password" v-model="password"></mt-field>
      <div class="checkbox-con">
        <b class="checkbox-bg active sprite-icon" n-type="checkbox" a-type="checked"></b>
        <a href="/v2/wap/protocal.html" class="protocal">同意太合用户注册协议</a>
      </div>
      <mt-button type="default" class="btn" size="large" @click='check'>注册</mt-button>
    </div>
  </div>
</div>
</template>
<script>
import 'mint-ui/lib/style.css'
import Vue from 'vue'
import axios from 'axios'
import '../../assets/font1/iconfont.css'
import {Field, Button, Toast} from 'mint-ui'
Vue.component(Field.name, Field)
Vue.component(Button.name, Button)
export default {
  data () {
    return {
      tel: '',
      code: '',
      adminCode: '',
      password: ''
    }
  },
  computed: {
    telState () {
      // 手机验证
      const str = '^1(3|4|5|6|7|8|9)\\d{9}$'
      if (this.tel.match(str)) {
        return 'success'
      } else {
        if (this.tel === '') {
          return ''
        } else {
          return 'error'
        }
      }
    },
    passwordState () {
      // 由数字、26个英文字母组成的字符串
      const str = '^[A-Za-z0-9]+$'
      if (this.password.match(str) && this.password.length >= 6) {
        return 'success'
      } else {
        if (this.password === '') {
          return ''
        } else {
          return 'error'
        }
      }
    }
  },
  methods: {
    back () {
      this.$router.go(-1)
    },
    sendCode () {
      axios.get('http://www.daxunxun.com/users/sendCode?tel=' + this.tel)
        .then(res => {
          // console.log(res.data)
          if (res.data === 1) {
            Toast({
              message: '改手机号已存在，请直接登录',
              position: 'bottom',
              duration: 2000
            })
          } else if (res.data === 0) {
            Toast({
              message: '获取验证码失败',
              position: 'bottom',
              duration: 2000
            })
          } else {
            this.adminCode = res.data.code
          }
        })
        .catch(err => console.log(err))
    },
    check () {
      if (this.telState !== 'success') {
        Toast({
          message: '手机号无效',
          position: 'bottom',
          duration: 2000
        })
        return
      }
      if (this.code !== '') {
        if (this.code !== this.adminCode) {
          Toast({
            message: '验证码错误',
            position: 'bottom',
            duration: 2000
          })
          return
        }
      } else {
        Toast({
          message: '请输入验证码',
          position: 'bottom',
          duration: 2000
        })
        return
      }
      if (this.passwordState !== 'success') {
        Toast({
          message: '密码无效',
          position: 'bottom',
          duration: 2000
        })
        return
      }
      this.register()
    },
    register () {
      axios.post('http://www.daxunxun.com/users/register', {
        username: this.tel,
        password: this.password
      }).then(res => {
        if (res.data === 2) {
          Toast({
            message: '用户已注册，请直接登录',
            position: 'bottom',
            duration: 2000
          })
        } else if (res.data === 0) {
          Toast({
            message: '注册失败',
            position: 'bottom',
            duration: 2000
          })
        } else {
          this.$router.push('/Login')
        }
      }).catch(err => console.log(err))
    }
  }
}
</script>
<style lang="scss">
@import '../../lib/reset.scss';
input::-webkit-input-placeholder {
        color: #ccc;
     }
.userForm{
  header{
      @include rect(100%,0.55rem);
    @include background-color(#f9f9f9);
    @include font-size(.18rem);
    @include position(relative);
    @include line-height(.55rem);
    @include text-align();
        .back{
        @include position(absolute);
        @include padding(0 0 0 .1rem);
        left:0;
          width: 30px;
        top: 5px;
        em{
          display:inline-block;
          @include rect(.11rem, .19rem);
          margin-top: 13px;
                    margin-left: 5px;
                    background: url(../../assets/icon.png) no-repeat -237px 0;
                    background-size: 305px;
          }
    }
      span{
        @include line-height(.55rem);
      }
  }
  .register{
    background:#fff;
    .form-wrapper{
      padding-top:.76rem;
    }
    .checkbox-con{
      @include rect(70%, auto);
      @include position(relative);
      @include padding(.08rem 0);
      @include margin(0 auto);
      margin-top:.2rem;
      .checkbox-bg{
        background-image: url("../../assets/icon.png");
        background-repeat: no-repeat;
        position: relative;
                display: inline-block;
                @include rect(.24rem, .14rem);
                left: 0;
                top: 1px;
                background-size: 610px 137px;
                background-position: -95px 0;
      }
      .active{
        background-position: -72px 0;
      }
      .protocal{
        @include text-color(#e13228);
        @include font-size(.16rem);
      }
    }
    .btn{
      @include rect(70%, .45rem);
      @include margin(0 auto);
      margin-top:.25rem;
      @include background-color(#e13228);
      @include text-color(#fff);
    }
  }
}
</style>
