<template>
    <div class="login_container">
        <h1>nCov-2019 疫情登记发布平台</h1>
        <div class="login_box">
            <!-- 头像 -->
            <div class="avatar_box">
                <img src="../assets/logo.png"/>
            </div>
            <!-- 输入框 -->
            <el-form ref="loginFormRef" class="login_form" :model="loginForm" :rules="login_rules">
                <el-form-item prop="username">
                    <el-input prefix-icon="iconfont icon-wode" placeholder="输入用户名" v-model="loginForm.username"></el-input>
                </el-form-item>
                <el-form-item prop="password">
                    <el-input prefix-icon="iconfont icon-password" type="password" placeholder="输入密码" v-model="loginForm.password"></el-input>
                </el-form-item>
            </el-form>
            <!-- 按钮 -->
            <el-row class="btns">
                <el-button @click="validateLoginForm()" type="primary">登录</el-button>
                <el-button @click="resetLoginForm()" type="info">重置</el-button>
            </el-row>

        </div>
    </div>
</template>

<script>
export default {
  data () {
    return {
      // 这是数据表单的绑定对象
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      // 这是验证部分
      login_rules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 11, message: '密码长度在 6 到 11 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    resetLoginForm () {
    //   console.log(this.$refs)
      this.$refs.loginFormRef.resetFields()
    },
    validateLoginForm () {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return
        const res = await this.$http.post('login', this.loginForm)
        // console.log(res.data.meta.status)
        if (res.data.meta.status !== 200) return this.$message.error('登录失败！')
        this.$message.success('登录成功！')
        console.log(res.data.data)
        // 存储token
        window.sessionStorage.setItem('token', res.data.data.token)
        this.$router.push('/home')
      })
    }
  }
}
</script>

<style lang='less' scoped>
.login_container{
    background-color: #2b4b6b;
    height: 100%;
    h1{
      text-align:center;
      color: #fff;
      margin: 0px;
      padding-top: 70px;
      font-size: 40px;
    }
    .login_box{
        width:450px;
        height:300px;
        background-color: #fff;
        border-radius: 3px;
        position: absolute;
        left: 50%;
        top: 55%;
        transform: translate(-50%, -50%);
        .avatar_box{
            width: 120px;
            height: 120px;
            border: 1px solid #eeeeee;
            border-radius: 50%;
            padding: 10px;
            box-shadow: 0px 5px 10px #eeeeee;
            position: relative;
            left: 50%;
            transform: translate(-50%,-50%);
            background-color: #fff;
            img{
                width: 100%;
                height:100%;
                border-radius: 50%;
                background-color: #dddddd;
            }
        }
        .login_form{
            position: relative;
            top: -10%;
            margin: 0 20px;
        }
        .btns{
            display: flex;
            float: right;
            position: relative;
            top: -10%;
            margin: 0 20px;
        }
    }
}
</style>
