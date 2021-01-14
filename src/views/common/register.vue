<template>
  <div>
    <el-card class="login-form-layout">
      <div class="brand-info">
        <h2 class="brand-info__text login-title">注册</h2>
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
            <el-button style="width: 45%" type="primary" @click.native.prevent="handleLogin">
              返回登录
            </el-button>
            <el-button style="width: 45%" type="primary" :loading="loading" @click.native.prevent="dataFormSubmit">
              注册
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
            url: this.$http.adornUrl('/sys/register'),
            method: 'post',
            data: this.$http.adornData({
              'username': this.dataForm.userName,
              'password': this.dataForm.password
            })
          }).then(({data}) => {
            console.log(data.code)
            if (data && data.code === 0) {
              this.$cookie.set('token', data.token)
              this.$router.push({path: '/login'})
              // this.$router.replace({name: 'home'})
            } else {
              this.$message.error(data.msg)
            }
          })
        }
      })
    },
    handleLogin () {
      this.$router.push({path: '/login'})
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
