<template>
  <div class="login_content">
    <div class="login_box">
      <!-- 头像区域 -->
      <div class="login_head">
        <img src="../assets/logo.png" alt="" srcset="" />
      </div>
      <!-- 登录表单区域 -->
      <el-form ref="loginFormRef" :model="loginForm" :rules="rules" label-width="0px" class="login-form-content">
        <!-- 用户名 -->
        <el-form-item  prop="username">
          <el-input v-model="loginForm.username" prefix-icon="iconfont icon-user"></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item  prop="password">
          <el-input type='password' v-model="loginForm.password" prefix-icon="iconfont icon-3702mima"></el-input>
        </el-form-item>
        <!-- 按钮区域 -->
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
        loginForm:{
            username:'admin',
            password:'123456'
        },
        rules: {
          username: [
            { required: true, message: '请输入用户名称', trigger: 'blur' },
            { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
          ],
          password:[
               { required: true, message: '请输入密码', trigger: 'blur' },
            { min: 6, max: 20, message: '长度在 6 到 20 个字符', trigger: 'blur' }
          ]
        }
    };
  },

  mounted() {},

  methods: {
      login(){
          this.$refs.loginFormRef.validate(async valided  => {
              if(!valided){
                  console.log('校验失败');
                  return;
              }
              const {data : res} = await this.$http.post('login',this.loginForm)
              console.log(res)
              if(res.meta.status != 200){
                  this.$message.error(res.meta.msg)
                  return;
              }else{
                   this.$message.success(res.meta.msg);
                   window.localStorage.setItem("token",res.data.token)
                   this.$router.push('/home')
              }
          });
      },
      resetLoginForm:function(  ){
          console.log(this);
          this.$refs.loginFormRef.resetFields();
      }
  },
};
</script>

<style lang="less" scoped>
.login_content {
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
}

.login_head {
  height: 130px;
  width: 130px;
  border: 1px solid #eee;
  border-radius: 50%;
  padding: 10px;
  box-shadow: 0 0 10px #ddd;
  position: absolute;
  background-color: #fff;
  left: 50%;
  transform: translate(-50%, -50%);
  img {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background-color: #eee;
  }
  
}
.login-form-content{
    position: absolute;
    width: 100%;
    padding: 0 10px;
    bottom: 0;
    box-sizing: border-box;
}
.btns{
      display: flex;
      justify-content: flex-end;
  }
</style>