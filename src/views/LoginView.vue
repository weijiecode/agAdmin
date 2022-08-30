<template>
    <div class="login">
        <!-- 登录 -->
        <img class="logoimg" src="../assets/logo1.png" alt="" />
        <div class="loginbox">
            <div class="login-title">欢迎访问</div>
            <div class="login-subtitle">请输入您的账户进行登录。</div>
            <div class="login-input">
                <el-form ref="loginFormRef" :model="loginForm" label-width="0px" class="login_form">
                    <el-form-item>
                        <el-input prefix-icon="el-icon-user" placeholder="请输入您的账号" v-model="loginForm.username">
                        </el-input>
                    </el-form-item>
                    <el-form-item>
                        <el-input prefix-icon="el-icon-lock" placeholder="请输入您的密码" v-model="loginForm.password"
                            show-password></el-input>
                    </el-form-item>
                    <el-form-item>
                        <el-radio v-model="radio" label="1">商家</el-radio>
                        <el-radio v-model="radio" label="2">管理员</el-radio>
                    </el-form-item>
                    <el-form-item class="btns">
                        <el-button style="width: 100%" type="primary" @click="login">登录</el-button>
                    </el-form-item>
                </el-form>
                <p class="nouser">还没有商家账号？
                <p @click="dialogVisible = true" class="subnouser">注册商家账号</p>
                </p>
            </div>
        </div>
        <!-- 注册弹框 -->
        <el-dialog title="商家账号实名注册" :visible.sync="dialogVisible" width="45%">
            <el-form status-icon label-width="100px" class="demo-ruleForm">
                <el-form-item label="用户名">
                    <el-input v-model="username"></el-input>
                </el-form-item>
                <el-form-item label="密码">
                    <el-input v-model="password"></el-input>
                </el-form-item>
                <el-form-item label="确认密码">
                    <el-input v-model="checkpassword"></el-input>
                </el-form-item>
                <el-form-item label="店铺名称">
                    <el-input v-model="shopname"></el-input>
                </el-form-item>
                <el-form-item label="身份证号码">
                    <el-input v-model="idcard"></el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="dialogVisible = false">取 消</el-button>
                <el-button type="primary" @click="regbtn">确认注册</el-button>
            </span>
        </el-dialog>
    </div>
</template>
  
  <script>
export default {
    data() {
        return {
            // 所有消息的id
            messageid: [],
            // 登录表单
            loginForm: {
                username: "",
                password: "",
            },
            // 选择登录用户类型 1商家 2管理员
            radio: '1',
            // 控制显示注册弹框的显示和隐藏
            dialogVisible: false,
            username: '',
            password: '',
            checkpassword: '',
            shopname: '',
            idcard: ''
        };
    },
    methods: {
        // 登录
        async login() {
            // 选择商家登录
            if (this.radio === '1') {
                const { data: res } = await this.$http.post("account/merchantlogin", {
                    username: this.loginForm.username,
                    password: this.loginForm.password
                });
                if (res.code === 200) {
                    this.$message.success("登录成功！");
                    localStorage.setItem("merchantid", res.data.id)
                    localStorage.setItem("shopname", res.data.shopname)
                    this.$router.push('merchant')
                } else {
                    if (res.code !== 200) return this.$message.error("账号或密码错误!");
                }
            }
            // 选择管理员登录
            if (this.radio === '2') {
                const { data: res } = await this.$http.post("account/adminlogin", {
                    username: this.loginForm.username,
                    password: this.loginForm.password
                });
                console.log(res);
                if (res.code === 200) {
                    this.$message.success("登录成功！");
                    localStorage.setItem("adminid", res.data.id)
                    this.$router.push('admin')
                } else {
                    if (res.code !== 200) return this.$message.error("账号或密码错误!");
                }
            }
        },
        // 确认注册
        async regbtn() {
            // 密码相同发送请求
            if (this.password == this.checkpassword) {
                const { data: res } = await this.$http.post("account/merregister", {
                    username: this.username,
                    password: this.password,
                    shopname: this.shopname,
                    idcard: this.idcard
                });
                console.log(res)
                if (res.code === 200) {
                    this.$message.success("注册成功！");
                    this.dialogVisible = false
                    // 注册成功后输入框都清空
                    this.username = ''
                    this.password = ''
                    this.checkpassword = ''
                    this.shopname = ''
                    this.cardid = ''
                } else {
                    this.$message.error("注册失败，请重试");
                }
            }else {
                this.$message.error("密码两次不一致，请重试");
            }
        }
    }
}
</script>
  
  <style scoped>
  .login {
      color: rgb(96, 98, 102);
      background-color: #f3f6fe;
      height: 100%;
      width: 100%;
      display: flex;
      flex-direction: column;
      align-items: center;
  }
  
  .logoimg {
      box-shadow: 0 0 10px #e6ecfa;
      margin-top: 30px;
      width: 60px;
      height: 60px;
      border-radius: 12px;
  }
  
  .loginbox {
      background-color: white;
      border-radius: 12px;
      box-shadow: 0 0 10px #e6ecfa;
      margin-top: 32px;
      width: 400px;
  }
  
  .login-title {
      font-size: 24px;
      font-weight: 600;
      margin-top: 36px;
      text-align: center;
  }
  
  .login-subtitle {
      color: #aaa;
      font-size: 14px;
      margin-top: 12px;
      padding: 0 12px;
      text-align: center;
  }
  
  .login-input {
      margin: 32px 40px;
  }
  
  .nouser {
      font-size: 14px;
      float: left;
      cursor: default;
      color: #aaa;
      margin-bottom: 30px;
  }
  
  .subnouser {
      color: #5da7f1;
      cursor: pointer;
      float: left;
      font-size: 14px;
  }
  
  .subpwd {
      color: #5da7f1;
      cursor: pointer;
      float: right;
      font-size: 14px;
      margin-top: 0;
  }
  
  .forgestpwd {
      margin-left: 260px;
  }
  
  
  
  .btn {
      width: 400px;
      display: flex;
      align-items: center;
      flex-direction: column;
  }
  
  .btn1 {
      width: 400px;
      display: flex;
      justify-content: center;
  }
  </style>