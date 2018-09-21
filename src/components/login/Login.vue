<template>
  <div class="userForm">
    <header>
      <a class="back" @click='back'><em></em></a>
      <span class="span1">账号登录</span>
      <span class="span2" @click='gopage("/register")'>注册</span>
    </header>
    <div class="login">
      <div class="form-wrapper register-wrapper">
        <mt-field placeholder="请输入手机号码" v-model="tel" type="tel" :state="telState"></mt-field>
        <mt-field placeholder="请输入密码" :state='passwordState' type="password" v-model="password"></mt-field>
        <mt-button type="default" class="btn" size="large" @click='check'>登录</mt-button>
      </div>
      <div class="t_other_box">
        <div class="t_other_header">
          <span>其他方式登录</span>
        </div>
      </div>
      <div class="t_other">
        <div class="auth-login-box">
          <a href="javascript:void(0);" class="baidu-login-btn">
            <i></i>
            <span>百度</span>
          </a>
        </div>
        <div class="auth-login-box">
          <a href="javascript:void(0);" class="qq-login-btn">
            <i></i>
            <span>QQ</span>
          </a>
        </div>
        <div class="auth-login-box">
          <a href="javascript:void(0);" class="wb-login-btn">
            <i></i>
            <span>微博</span>
          </a>
        </div>
      </div>
      <div class="login-footer">
        <p style="position: relative;">阅读
          <a @click='gopage("/protocol")'>太合用户注册协议</a>
        </p>
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
      // 由数字/26个英文字母组成的字符串
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
      this.login()
    },
    login () {
      axios.post('http://www.daxunxun.com/users/login', {
        username: this.tel,
        password: this.password
      }).then(res => {
        // console.log(res.data)
        if (res.data === 2) {
          Toast({
            message: '用户未注册',
            position: 'bottom',
            duration: 2000
          })
        } else if (res.data === 0) {
          Toast({
            message: '失败失败',
            position: 'bottom',
            duration: 2000
          })
        } else if (res.data === -1) {
          Toast({
            message: '密码错误',
            position: 'bottom',
            duration: 2000
          })
        } else {
          localStorage.setItem('username', this.tel)
          this.$router.push('/recommend')
        }
      }).catch(err => console.log(err))
    },
    gopage (path) {
      this.$router.push(path)
    }
  }
}
</script>
<style lang="scss" >
@import '../../lib/reset.scss';
#tab{
  display:none;
}
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
                    background: url(//static3.qianqian.com/m-tpass/img/icon.png) no-repeat -237px 0;
                    background-size: 305px;
          }
    }
      span{
        @include line-height(.55rem);
      }
      .span2{
      float:right;
      margin-right:.1rem;
    }
  }
  .login{
    background:#fff;
    .form-wrapper{
      padding-top:.76rem;
        .btn{
      @include rect(70%, .45rem);
      @include margin(0 auto);
      margin-top:.25rem;
      @include background-color(#e13228);
      @include text-color(#fff);
        }
    }
    .t_other_box{
      margin-top:.5rem;
      .t_other_header{
        margin:0 .75rem;
          border-top: 1px solid #d0d0d0;
        @include text-align();
        span{
          @include text-color(#999);
          top:-12px;
          position:relative;
          @include font-size(12px);
          @include background-color(#fff);
          @include padding(.03rem .1rem);
        }
      }
    }
    .t_other{
            padding: 0 50px;
            box-sizing: border-box;
            margin-top: 12px;
      @include flexbox();
      .auth-login-box{
        @include flex();
        @include text-align();
        a{
          display:inline-block;
          @include text-color(#000);
          i{
            background-repeat: no-repeat;
                        background-image: url('../../assets/icon_new.png');
                        background-size: 200px 150px;
            display:inline-block;
            @include rect(.41rem, .41rem);
          }
          span{
            display:block;
            @include font-size(12px);
            @include text-align();
          }
        }
        .baidu-login-btn{
          i{
            background-position: 0 0;
          }
        }
        .qq-login-btn{
          i{
            background-position: -89px 0;
          }
        }
        .wb-login-btn{
          i{
            background-position: -133px 0;
          }
        }
      }
    }
    .login-footer{
      margin-top:38%;
      width:100%;
      @include text-align();
      @include text-color(#666);
      @include font-size(.12rem);
    }
  }
}
</style>
