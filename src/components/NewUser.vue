<template>
  <div>
    <input v-model="email" type="text" placeholder="Email" />
    <input v-model="login" type="text" placeholder="Логин" />
    <input v-model="password" type="password" placeholder="Пароль" />
    <button @click="setLogin">Зарегаться</button>
  </div>
</template>

<script>
  import $ from 'jQuery'
  export default {
    name: 'NewUser',
    data() {
      return {
        login: '',
        password: '',
        email: '',
      }
    },
    methods: {
      setLogin() {
        $.ajax({
          url: "http://127.0.0.1:8000/auth/users/",
          type: "POST",
          data: {
            username: this.login,
            password: this.password,
            email: this.email
          },
          success: (response) => {
            console.log(response.data.attributes.auth_token)
            alert("Спасибо за регистрацию")
            localStorage.setItem("auth_token", response.data.attributes.auth_token)
            this.$router.push({ name: "Login" })
          },
          error: (response) => {
            if (response.status === 400) {
              alert("Проверь логин или пароль")
            }  
          }
        })
      },
    }
  }
</script>

<style>
</style>
