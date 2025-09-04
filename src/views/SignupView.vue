<template>
<!-- sign up -->
<div id="signUpPage" class="bg-yellow">
<div class="conatiner signUpPage vhContainer">
    <div class="side">
    <a href="#"><img class="logoImg" src="https://raw.githubusercontent.com/hexschool/2022-web-layout-training/main/todolist/logo.png" alt=""></a>
    <img class="d-m-n" src="https://raw.githubusercontent.com/hexschool/2022-web-layout-training/main/todolist/img.png" alt="workImg">
    </div>
    <div>
    <form class="formControls" action="index.html">
        <h2 class="formControls_txt">註冊帳號</h2>
        <label class="formControls_label" for="email">Email</label>
        <input class="formControls_input" type="text" id="email" name="email" placeholder="請輸入 email" required v-model="signupField.email">
        <label class="formControls_label" for="name">您的暱稱</label>
        <input class="formControls_input" type="text" name="name" id="name" placeholder="請輸入您的暱稱" v-model="signupField.nickname">
        <label class="formControls_label" for="pwd">密碼</label>
        <input class="formControls_input" type="password" name="pwd" id="pwd" placeholder="請輸入密碼" required v-model="signupField.password">
        <label class="formControls_label" for="pwd">再次輸入密碼</label>
        <input class="formControls_input" type="password" name="pwd" id="pwd" placeholder="請再次輸入密碼" required v-model="signupField.reCheckPassword">
        <input class="formControls_btnSubmit" type="button" value="註冊帳號" @click="signup()">
        <RouterLink to="/login" class="formControls_btnLink">登入</RouterLink>
    </form>
    </div>
</div>
</div>
</template>

<script setup>
  import { ref } from 'vue';
  import axios from 'axios';

  const apiBaseUrl = 'https://todolist-api.hexschool.io/';
  const signupField = ref({
    email: '',
    password: '',
    reCheckPassword: '',
    nickname: ''
  })
  const signup = async() => {
    if (signupField.value.reCheckPassword!=signupField.value.password) {
      alert("密碼不一致，請重新輸入")
      signupField.reCheckPassword = ''
      return
    }
    try{
        const res = await axios.post(`${apiBaseUrl}users/sign_up`, signupField.value);
        console.log(res);
        if (res.data.status) {
            alert("註冊成功");
        }
    }catch(error){
        console.log(error);
        const errorMsg = error.response.data.message;
        alert(errorMsg);
    }
  }
</script>