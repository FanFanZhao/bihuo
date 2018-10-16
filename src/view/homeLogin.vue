<template>
    <div class="homeLogin">
        <div class="loginBox" v-if="!token">
            <div class="title">登录</div>
            <div class="userName">
                <input type="text" placeholder="用户名" v-model="account">
            </div>
            <div class="password">
                <input type="password" placeholder="密码" v-model="password">
            </div>
            <div class="login-btn redBg" @click="login">登录</div>
            <div class="forget">
                <router-link tag="span" to="/forgetPwd">忘记密码</router-link>
                <router-link tag="span" to="/components/register">注册</router-link>
                
            </div>
        </div>
        <div v-if="token" class="userInfo">
            <p class="title">欢迎，{{account}}</p>
            <div class="info">
                <div>
                    <span>账号ID:</span>
                    <span>{{userId}}</span>
                </div>
                <div>
                    <span>当前等级:</span>
                    <span>{{level}}</span>
                </div>
                <div>
                    <span>交易费折扣:</span>
                    <span>{{off}}</span>
                </div>
                <div>
                    <span>资金总估:</span>
                    <span>{{total}}</span>
                </div>
            </div>
            <div class="btns">
                <span>充值</span>
                <span>提现</span>
                <span>财务中心</span>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  // name:'homeLogin',
  data() {
    return {
      hasLogin: false,
      account: "",
      password:'',
      userId: "",
      level: "",
      off: "无",
      total: "",
      token:''
    };
  },
  created(){
      this.token = window.localStorage.getItem('token')||'';
    //   this.account = window.localStorage.getItem('accountNum')||'';
      this.userInfo();
  },
  methods: {
    userInfo() {
        if(this.token == ''){return}
      this.$http({
        url: "/api/" + "user/info",
        method: "get",
        data: {},
        headers: { Authorization: localStorage.getItem("token") }
      })
        .then(res => {
          // console.log(res);
          if (res.data.type == "ok") {
            localStorage.setItem("user_id", res.data.message.id);
            console.log(res);
            
            this.account = res.data.message.phone;
            this.userId = res.data.message.id;
          }
        })
        .catch(error => {});
    },
    login() {
      let account_number = this.$utils.trim(this.account);
      let password = this.$utils.trim(this.password);
      if (this.account.length == "") {
        layer.tips("请输入账号!", "#account");
        return;
      }
      if (this.password.length < 6) {
        layer.tips("密码不能小于六位!", "#pwd");
        return;
      }
      this.$http({
        url: this.$utils.laravel_api + "user/login",
        method: "post",
        data: {
          user_string: account_number,
          password: password,
          type: 1
        }
      })
        .then(res => {
          console.log(res);

          res = res.data;
          if (res.type === "ok") {
              this.token = res.message;
              
              eventBus.$emit('toHeader',{token:res.message,account:this.account});
            localStorage.setItem("token", res.message);
            localStorage.setItem("accountNum", account_number);

            this.$store.commit("setAccountNum");
            this.userInfo();
            // this.$router.push("/");
          } else {
            layer.msg(res.message);
          }
        })
        .catch(error => {
          console.log(error);
        });
    }
  }
};
</script>

<style lang='scss'>
.homeLogin {
  position: absolute;
  z-index: 9;
  background: rgba(255, 255, 255, 0.7);

  top: 60px;
  right: 200px;
  font-size: 14px;
  > div {
    padding: 0 20px 20px;
    width: 300px;
    height: 280px;
  }
  &:hover {
    background: rgba(255, 255, 255, 0.9);
  }
  .title {
    font-size: 20px;
    line-height: 60px;
  }
  .loginBox {
    input {
      line-height: 38px;
      border: 1px solid #ccc;
      padding: 0 16px;
      margin-bottom: 20px;
      width: 100%;
    }
    .login-btn {
      cursor: pointer;
      line-height: 36px;
      text-align: center;
    }
    .forget {
      display: flex;
      justify-content: space-between;
      font-size: 14px;
      line-height: 30px;
      span {
        cursor: pointer;
      }
    }
  }
  .userInfo {
    background: rgba(255, 255, 255, 0.9);
    .info {
      span:last-child {
        color: #d45858;
        line-height: 30px;
      }
    }
    .btns {
      display: flex;
      margin-top: 30px;
      border-top: 1px solid #ccc;
      padding-top: 15px;
      justify-content: space-between;
      span {
        line-height: 30px;
        background: #d45858;
        color: #fff;
        cursor: pointer;
        width: 80px;
        font-size: 12px;
        text-align: center;
      }
    }
  }
}
</style>
