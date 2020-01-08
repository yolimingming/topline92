<template>

<div class="login-container">

  <div class="login-box">
    <el-form ref="loginFormRef" :model="loginForm" :rules="loginFormRules">
        <img src="./logo_index.png" alt="">
    <el-form-item prop="mobile">
      <el-input v-model="loginForm.mobile" placeholder="请输入手机号"></el-input>
    </el-form-item>
     <el-form-item prop="code">
      <el-input v-model="loginForm.code" placeholder="请输入验证码"></el-input>
     </el-form-item>
      <el-form-item style="text-align:left;" prop="xieyi">
       <el-checkbox v-model="loginForm.xieyi"></el-checkbox>
       <span>我已阅读并同意</span>
       <a href="#">用户协议</a>和<a href="#">隐私条款</a>
     </el-form-item>
     <el-form-item>
       <el-button type="primary" style="width:100%" @click="login()">登录</el-button>
     </el-form-item>
    </el-form>
   </div>
</div>

</template>

<script>
export default {
  name: '',
  data () {
    var xieyiTest = function (rule, value, callback) {
      value ? callback() : callback(new Error('请无条件遵守规矩'))
    }
    return {
      loginForm: {
        mobile: '',
        code: ''

      },
      loginFormRules: {
        mobile: [
          // 必填
          { required: true, message: '手机号码必填' },
          { pattern: /^1[35789]\d{9}$/, message: '手机号码格式不对' }
        ],
        code: [
          { required: true, message: '验证码必填' }
        ], // 验证码
        xieyi: [
          { validator: xieyiTest }
        ] // 协议复选框
      }
    }
  },
  methods: {
    login () {
      this.$refs.loginFormRef.validate(valid => {
        if (!valid) { return false }
        let pro = this.$http({
          url: '/mp/v1-0/authorizations',
          method: 'POST',
          data: this.loginForm
        })
        pro
          .then(result => {
            window.sessionStorage.setItem('userinfo', JSON.stringify(result.data.data))
            this.$router.push({ name: 'home' })
          })
          .catch(err => {
            this.$message.error('手机号码或验证码错误:' + err)
          })
      })
    }

  }

}
</script>

<style lang="less" scoped>
.login-container{
    height: 100%;
    width: 100%;
    background-image: url('./login_bg.jpg');
    display: flex;
    align-items: center;
    justify-content: center;
    .login-box{
        width: 410px;
        height: 340px;
        background-color: #fff;
        display: flex;
        justify-content: center;
        align-items: center;
        .el-form{
            width: 75%;
            text-align: center;
            img{
                width: 60%;
                margin: 20px auto;
            }
        }
    }
}
</style>
