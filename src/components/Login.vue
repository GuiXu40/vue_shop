<template>
  <div class="login_container">
      <div class="login_box">
          <div class="avator_box">
              <img src="../assets/logo.png" alt="">
          </div>

          <el-form ref="loginFormRef" :model="loginForm" :rules="loginFormRules" label-width="0px" class="login_form" >
              <!-- username -->
            <el-form-item prop="username">
                <el-input prefix-icon="el-icon-user" v-model="loginForm.username"></el-input>
            </el-form-item>
            <!-- password -->
            <el-form-item prop="password">
                <el-input prefix-icon="el-icon-lock" v-model="loginForm.password" type="password"></el-input>
            </el-form-item>
            <el-form-item class="btns">
                <el-button type="primary" @click="login">登录</el-button>
                <el-button type="info" @click="resetLoginForm">重置</el-button>
            </el-form-item>
          </el-form>
      </div>
  </div>
</template>

<script>
export default {
    data() {
        return {
            // 这是登陆表单的数据对象
            loginForm: {
                username: 'admin',
                password: '123456'
            },

            // 表单验证
            loginFormRules: {
                username: [
                    { required: true, message: '请输入用户名', trigger: 'blur' },
                    { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
                ],
                password: [
                    { required: true, message: '请输入密码', trigger: 'blur' },
                    { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }                   
                ]
            }
        }
    },
    methods: {
        // 点击重置表单
        resetLoginForm () {
            // console.log(this);
            this.$refs.loginFormRef.resetFields();
        },

        login () {
            this.$refs.loginFormRef.validate(async (valid) => {
                if (!valid) return;
               const {data:res} = await this.$http.post('login', this.loginForm );
            //    console.log(res);
                if (res.meta.status != 200) {
                    // console.log("登录失败！！！");
                    return this.$message.error("登录失败！！！");
                }
                else {
                    // console.log("登录成功！！！");
                    this.$message.success("登录成功！！！");

                    // 1. 将登录成功之后的token， 保存到客户端的 sessionStorage 中
                    //  1.1 项目中除了登录之外的其他API接口， 必须在登录之后才能访问
                    //  1.2 token只因在当前网站打开期间有效， 所以讲token保存在sessionStorage中
                    console.log(res);
                    window.sessionStorage.setItem('token', res.data.token);
                    // 2. 通过编程式导航跳到后台主页， 路由地址是 /home
                    this.$router.push('/home');
                }
            })
        }
    },
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
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);

    .avator_box {
        height: 130px;
        width: 130px;
        border: 1px solid #eee;
        border-radius: 50%;
        padding: 10px;
        box-shadow: 0 0 10px #ddd;
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