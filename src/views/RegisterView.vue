<template>
  <div class="homeBox">
    <div class="container">
      <div class="sign-box">
        <div class="apple-btn sign-apple">
          <li class="red-btn"></li>
          <li class="yellow-btn"></li>
          <li class="green-btn"></li>
        </div>
        <div class="title">Sign</div>
        <form @submit.prevent="sign">
          <div class="input">
            <input v-model="username" type="text" id="sign-user"
              placeholder="Have A Good Name?">
          </div>
          <div class="input">
            <input v-model="password" type="password" id="sign-password"
              placeholder="Keep Secret">
          </div>
          <div class="input">
            <input v-model="password_confirm" type="password"
              id="password_confirm" placeholder="Confirm Your Password">
            <div class="error-message">{{ error_message }}</div>
          </div>
          <button type="submit" class="btn sign-btn">Sign up</button>
        </form>
        <div class="change-box sign-change">
          <div class="change-btn toLogin" @click="GotoLogin">
            <span>Login</span>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import { ref } from 'vue';
import { useStore } from 'vuex';
import router from '@/router';
import $ from "jquery"
export default {
  name: 'RegisterView',
  components: {
  },
  setup() {
    const store = useStore();
    let username = ref('');
    let password = ref('');
    let error_message = ref('');
    let password_confirm = ref('');
    const sign = () => {
      error_message.value = "";
      $.ajax({
        url: "https://app165.acapp.acwing.com.cn/myspace/user/",
        type: "POST",
        data: {
          username: username.value,
          password: password.value,
          password_confirm: password_confirm.value,
        },
        success(resp) {
          console.log(resp);
          if (resp.result === "success") {
            store.dispatch("login", {
              username: username.value,
              password: password.value,
              success() {
                router.push({ name: 'userlist' });
              },
              error() {
                error_message.value = "System Error";
              }
            });
          } else error_message.value = resp.result;
          //console.log(resp);
        }
      });
      // console.log(store);
      //console.log(username.value,password.value,password_confirm.value);
    };
    const GotoLogin = () => {
      router.push({
        name: "login",
      });
    }
    return {
      username,
      password,
      password_confirm,
      error_message,
      sign,
      GotoLogin,
    }
  }
}
</script>



<style scoped>
* {
  padding: 0px;
  margin: 0px;
}

.error-message {
  display: flex;
  font-weight: 800;
  color: red;
  font-size: 1em;
  justify-content: flex-start;
}

.homeBox {
  /*! autoprefixer: off */
  /*  background: -webkit-gradient(linear,
      100% 0,
      0 0,
      from(#1e2a78),
#485296
#28389e
#14248b,#fbc531
#1d428a,#fdb927
      to(#FF2E4c));*/
  position: fixed;
  width: 100%;
  height: 100%;
  top: 60px;
  background: -webkit-linear-gradient(130deg, #ffc72c, #14248b);
}

.container {
  position: absolute;
  height: 430px;
  width: 600px;
  background-color: rgba(255, 255, 255, .9);
  left: 50%;
  top: 30%;
  transform: translate(-50%, -30%);
  border-radius: 10px;
  box-shadow: 0px 0px 10px rgba(17, 39, 59, 0.8);
  border: 1px solid rgba(17, 39, 59, 1);
  box-sizing: border-box;
}

.container:hover .title {
  font-size: 20px;
  /* transform: translate(0,-30%); */
}

.container:hover input {
  transform: translate(0, -30%);
}

.container:hover .btn {
  height: 30px;
  width: 90px;
  transform: translate(0, -30%);
  font-size: 12px;
}

.container:hover .change-box {
  height: 200px;
}

.container:hover .change-btn {
  display: block;
}

.title {
  margin-top: 10px;
  position: relative;
  height: 40px;
  width: 100%;
  font-size: 30px;
  display: flex;
  justify-content: center;
  align-items: center;
  text-transform: uppercase;
  font-weight: 500;
  letter-spacing: 3px;
  transition: .4s;
}

.input {
  width: 400px;
  height: 45px;
  position: relative;
  margin: 40px auto;
  /* border-radius: 45px;
    overflow: hidden; */
}

input {
  position: relative;
  width: 100%;
  height: 100%;
  border: none;
  outline: none;
  /* box-sizing: border-box; */
  padding-left: 15px;
  font-size: 16px;
  background-color: rgba(255, 255, 255, 0.4);
  border-radius: 45px;
  transition: .4s;
}

.btn {
  height: 50px;
  width: 160px;
  position: relative;
  margin: -10px auto;
  background-color: rgba(0, 0, 0, 0.1);
  border-radius: 20px;
  color: rgba(255, 255, 255, .4);
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  transition: .4s;
}

.change-box {
  position: absolute;
  height: 0px;
  width: 100%;
  clip-path: polygon(100% 50%, 50% 100%, 100% 100%);
  bottom: 0px;
  transition: .4s;
}

.change-btn {
  position: absolute;
  bottom: 30px;
  right: 40px;
  font-size: 20px;
  display: none;
  font-weight: 500;
}

.change-btn:hover {
  text-shadow: 0px 0px 3px rgba(200, 200, 200, .8);
  cursor: pointer;
}

.login-box input {
  caret-color: white;
  color: rgba(255, 255, 255, 0.8);
}

.sign-change {
  background-color: rgba(17, 39, 59, 0.8);
}

.toLogin {
  color: white;
}

.sign-box input {
  box-shadow: 0 0 3px black;
}

.sign-box .btn {
  color: #1e90ff;
  background-color: rgba(200, 200, 200, .1);
  transition: .5s;
}

.sign-box .btn:hover {
  color: white;
  background-color: #1e90ff;
}

/* Mac 按钮样式及特效 */
.apple-btn {
  position: absolute;
  height: 15px;
  width: 65px;
  top: 3px;
}

.apple-btn li {
  list-style: none;
  position: relative;
  height: 15px;
  width: 15px;
  border-radius: 15px;
  opacity: 0;
}

.sign-apple li {
  right: 5px;
  float: right;
}

.sign-apple {
  right: 5px;
}

li:nth-child(2) {
  margin: 0px 2px
}

.red-btn {
  background-color: red;
  transition: .3s;
  transform: translate(0, -50%);
}

.yellow-btn {
  background-color: orange;
  /* transition-delay: .2s; */
  transition: .6s;
  transform: translate(0, -50%);
}

.green-btn {
  background-color: rgb(15, 136, 15);
  /* transition-delay: .3s; */
  transition: .9s;
  transform: translate(0, -50%);
}

.container:hover .red-btn {
  opacity: 1;
  transform: translate(0, 0);
}

.container:hover .yellow-btn {
  opacity: 1;
  transform: translate(0, 0);
}

.container:hover .green-btn {
  opacity: 1;
  transform: translate(0, 0);
}
</style>