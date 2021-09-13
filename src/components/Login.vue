<template>
  <div class="login_container">
    <!-- Portrai -->
    <div class="login_box">
      <div class="avatar_box">
        <img
          src="../assets/logo.png"
          alt=""
        >
      </div>
      <!-- Login Form -->
      <el-form
        label-width="0px"
        class="login_form"
        :model="loginForm"
        :rules="loginFormRules"
        ref="loginFormRef"
      >
        <!-- Username Field -->
        <el-form-item prop="username">
          <el-input
            v-model="loginForm.username"
            prefix-icon="iconfont icon-user"
          ></el-input>
        </el-form-item>
        <!-- Password Field -->
        <el-form-item prop="password">
          <el-input
            v-model="loginForm.password"
            prefix-icon="iconfont icon-3702mima"
            type="password"
          ></el-input>
        </el-form-item>
        <!-- Button Field -->
        <el-form-item class="btns">
          <el-button
            type="primary"
            @click="login"
          >Login</el-button>
          <el-button
            type="info"
            @click="resetLoginForm()"
          >Reset</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      loginFormRules: {
        username: [
          { required: true, message: 'Please input username', trigger: 'blur' },
          { min: 3, max: 5, message: 'The length should be between 3 to 5 chars', trigger: 'blur' }
        ],
        password: [
          { required: true, message: 'Please input password', trigger: 'blur' },
          { min: 6, max: 15, message: 'The length should be between 6 to 15 chars', trigger: 'blur' }
        ]
      },
      loginFormRef: {}
    }
  },

  methods: {
    resetLoginForm() {
      this.$refs.loginFormRef.resetFields()
    },

    login() {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)

        if (res.meta.status !== 200) {
          return this.$message('Login failed')
        }
        this.$message('Login succeeded')
        // save token to sessionStorage
        window.sessionStorage.setItem('token', res.data.token)
        // navigate to backend home
        this.$router.push('/home')
      })
    }
  }
}
</script>

<style lang="less" scoped>
.login_container {
  background-color: #2b4b6b;
  height: 100%;
}

.login_box {
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 3px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);

  .avatar_box {
    height: 130px;
    width: 130px;
    border: 1px solid #eee;
    border-radius: 50%;
    padding: 10px;
    box-shadow: 0 0 10x #ddd;
    position: absolute;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;

    img {
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background-color: #eee;
    }
  }
}

.login_form {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}

.btns {
  display: flex;
  justify-content: flex-end;
}
</style>
