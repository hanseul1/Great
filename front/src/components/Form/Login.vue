<template>
  <div>
    <h1>Login</h1>
  <div class="login-subcontainer">
    
    <div class="login-container">
      <div class="login-box">
        <div class="input-with-label">
          <label for="loginID">ID</label>
          <input v-model="loginID" id="loginID" @keyup.enter="loginApi" type="text" />
        </div>
        <br />
        <div class="input-with-label">
          <label for="loginPW">PW</label>
          <input v-model="loginPW" type="password" @keyup.enter="loginApi" id="loginPW" />
        </div>
      </div>
      <div style="width: 100%; display: relative; margin-top: 5px;">
        <!-- <div style="width: 50%; float: left; margin-left: 2px; margin-top: 7px;">
          <input type="checkbox" v-model="remID" id="remID" /> 아이디 저장하기
        </div> -->
        <div style="float: right;">
          <v-btn rounded color="#ff7761" dark @click="loginApi">확 인</v-btn>
        </div>
      </div>
    </div>
    <hr />
    <div class="sns-login">
      <div style="width: 50%; float: left;">
        <h3>SNS 로그인</h3>
      </div>
      <div style="width: 50%; float: right;">
        <SocialLogin />
      </div>
    </div>
    <div class="add-option">
      <div class="wrap">
        <p>비밀번호를 잊으셨나요?</p>
        <button @click="findPassword" class="btn--text">비밀번호 찾기</button>
      </div>
      <div class="wrap">
        <p>아직 회원이 아니신가요?</p>
        <button @click="joinRequest" class="btn--text">회원가입</button>
      </div>
    </div>
  </div>
  </div>
</template>

<script>
/* eslint-disable no-unused-vars */
import "@/assets/style/css/authStyle.css";
import axios from "axios";
import UserApi from "@/apis/UserApi";
import SocialLogin from "@/components/common/SocialLogin.vue";

export default {
  components: {
    SocialLogin
  },
  methods: {
    async loginApi() {
      let { remID, loginID, loginPW } = this;
      await UserApi.requestLogin(loginID, loginPW, res => {
        console.log(res);
      });
      this.tokenApi();
    },
    tokenApi() {
      var token = sessionStorage.getItem("token");
      if (token != 'null') {
        this.$router.push('/')
        window.location.reload()
      } else {
        alert("존재하지 않는 이메일이거나, 틀린 비밀번호입니다.")
      }
    },
    joinRequest() {
      this.$router.push("Join");
    },
    findPassword(){
      this.$router.push("FindPassword");
    }
  },

  data() {
    return {
      remID: false,
      loginID: "",
      loginPW: "",
      join: false,
      component: this
    };
  }
};
</script>