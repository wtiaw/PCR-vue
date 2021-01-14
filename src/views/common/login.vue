<template>
  <div>
    <el-card class="login-form-layout">
      <div class="brand-info">
        <h2 class="brand-info__text login-title">登录</h2>
      </div>
      <div class="login-main">
        <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()"
                 status-icon>
          <el-form-item prop="userName">
            <el-input v-model="dataForm.userName" placeholder="帐号">
                <span slot="prefix">
                  <icon-svg name="user" class="color-main"></icon-svg>
                </span>
            </el-input>
          </el-form-item>
          <el-form-item prop="password">
            <el-input v-model="dataForm.password" type="password" placeholder="密码">
                <span slot="prefix">
                  <icon-svg name="password"></icon-svg>
                </span>
            </el-input>
          </el-form-item>
          <el-form-item style="margin-bottom: 60px;text-align: center">
            <el-button style="width: 45%" type="primary" :loading="loading" @click.native.prevent="dataFormSubmit">
              登录
            </el-button>
            <el-button style="width: 45%" type="primary" @click.native.prevent="handleRegister">
              前往注册
            </el-button>
          </el-form-item>
        </el-form>
      </div>
    </el-card>
  </div>
</template>

<script>
export default {
  data () {
    return {
      dataForm: {
        userName: '',
        password: ''
      },
      dataRule: {
        userName: [
          {required: true, message: '帐号不能为空', trigger: 'blur'}
        ],
        password: [
          {required: true, message: '密码不能为空', trigger: 'blur'}
        ]
      },
      loading: false
    }
  },
  created () {
  },
  methods: {
    // 提交表单
    dataFormSubmit () {
      this.$refs['dataForm'].validate((valid) => {
        if (valid) {
          this.$http({
            url: this.$http.adornUrl('/sys/login'),
            method: 'post',
            data: this.$http.adornData({
              'username': this.dataForm.userName,
              'password': this.dataForm.password
            })
          }).then(({data}) => {
            if (data && data.code === 0) {
              this.$cookie.set('token', data.token)
              this.$router.replace({name: 'home'})
            } else {
              this.$message.error(data.msg)
            }
          })
        }
      })
    },
    handleRegister () {
      this.$router.push({path: '/register'})
    }
  }
}
</script>

<style scoped>
.login-form-layout {
  position: absolute;
  left: 0;
  right: 0;
  width: 360px;
  margin: 140px auto;
  border-top: 10px solid #409EFF;
}

.login-title {
  text-align: center;
}

.login-center-layout {
  background: #409EFF;
  width: auto;
  height: auto;
  max-width: 110%;
  max-height: 100%;
  margin-top: 190px;
}
</style>

<!--<style lang="scss">-->
<!--.site-wrapper.site-page&#45;&#45;login {-->
<!--  position: absolute;-->
<!--  top: 0;-->
<!--  right: 0;-->
<!--  bottom: 0;-->
<!--  left: 0;-->
<!--  background-color: rgba(38, 50, 56, .6);-->
<!--  overflow: hidden;-->

<!--  &:before {-->
<!--    position: fixed;-->
<!--    top: 0;-->
<!--    left: 0;-->
<!--    z-index: -1;-->
<!--    width: 100%;-->
<!--    height: 100%;-->
<!--    content: "";-->
<!--    background-image: url(~@/assets/img/login_bg.jpg);-->
<!--    background-size: cover;-->
<!--  }-->

<!--  .site-content__wrapper {-->
<!--    position: absolute;-->
<!--    top: 0;-->
<!--    right: 0;-->
<!--    bottom: 0;-->
<!--    left: 0;-->
<!--    padding: 0;-->
<!--    margin: 0;-->
<!--    overflow-x: hidden;-->
<!--    overflow-y: auto;-->
<!--    background-color: transparent;-->
<!--  }-->

<!--  .site-content {-->
<!--    min-height: 100%;-->
<!--    padding: 30px 500px 30px 30px;-->
<!--  }-->

<!--  .brand-info {-->
<!--    margin: 220px 100px 0 90px;-->
<!--    color: #fff;-->
<!--  }-->

<!--  .brand-info__text {-->
<!--    margin: 0 0 22px 0;-->
<!--    font-size: 48px;-->
<!--    font-weight: 400;-->
<!--    text-transform: uppercase;-->
<!--  }-->

<!--  .brand-info__intro {-->
<!--    margin: 10px 0;-->
<!--    font-size: 16px;-->
<!--    line-height: 1.58;-->
<!--    opacity: .6;-->
<!--  }-->

<!--  .login-main {-->
<!--    position: absolute;-->
<!--    top: 0;-->
<!--    right: 0;-->
<!--    padding: 150px 60px 180px;-->
<!--    width: 470px;-->
<!--    min-height: 100%;-->
<!--    background-color: #fff;-->
<!--  }-->

<!--  .login-title {-->
<!--    font-size: 16px;-->
<!--  }-->

<!--  .login-captcha {-->
<!--    overflow: hidden;-->

<!--    > img {-->
<!--      width: 100%;-->
<!--      cursor: pointer;-->
<!--    }-->
<!--  }-->

<!--  .login-btn-submit {-->
<!--    width: 100%;-->
<!--    margin-top: 38px;-->
<!--  }-->
<!--}-->
<!--</style>-->
