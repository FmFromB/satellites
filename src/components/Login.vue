<template>
  <mu-container>
    <mu-form ref="form" :model="validateForm" class="mu-demo-form">
      <mu-form-item label="Логин" help-text="help text" prop="username">
        <mu-text-field v-model="validateForm.username" prop="username"></mu-text-field>
      </mu-form-item>
      <mu-form-item label="Пароль" prop="password">
        <mu-text-field type="password" v-model="validateForm.password" prop="password"></mu-text-field>
      </mu-form-item>
      <mu-form-item>
        <mu-button color="primary" @click="setLogin">submit</mu-button>
        <mu-button @click="clear">reset</mu-button>
      </mu-form-item>
    </mu-form>
  </mu-container>

</template>

<script>
  import $ from 'jQuery'
  export default {
    name: 'Login',
    data() {
      return {
        validateForm: {
          username: '',
          password: '',
        }
      }
    },
    methods: {
      submit() {
        this.$refs.form.validate().then((result) => {
          console.log('form valid: ', result)
        });
      },
      clear() {
        this.$refs.form.clear();
        this.validateForm = {
          username: '',
          password: '',
          isAgree: false
        };
      },
      setLogin() {
        $.ajax({
          url: "http://127.0.0.1:8000/auth/token/login",
          type: "POST",
          data: {
            username: this.validateForm.username,
            password: this.validateForm.password
          },
          success: (response) => {
            console.log(response.data.attributes.auth_token)
            alert("Добро пожаловать")
            localStorage.setItem("auth_token", response.data.attributes.auth_token)
            this.$router.push({ name: "Home" })
          },
          error: (response) => {
            if (response.status === 400) {
              alert("перепроверь логин или пароль")
            }  
          }
        })
      },
    }
  }
</script>

<style>
</style>
