<template>
     <div id="app">
    <div class="container">
      <div class="row justify-content-center">
        <h1 class="h3 mb-3 font-weight-normal">
          請先登入
        </h1>
        <div class="col-8">
          <form id="form" class="form-signin">
            <div class="form-floating mb-3">
              <input type="email" class="form-control" id="username" placeholder="name@example.com" required autofocus>
              <label for="username">Email address</label>
            </div>
            <div class="form-floating">
              <input type="password" class="form-control" id="password" placeholder="Password" required>
              <label for="password">Password</label>
            </div>
            <button class="btn btn-lg btn-primary w-100 mt-3" type="submit" @click.prevent="login">
              登入
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const { VITE_APP_URL } = import.meta.env

export default {
  data () {
    return {

    }
  },
  methods: {
    login () {
      const userData = {
        username: document.querySelector('#username').value,
        password: document.querySelector('#password').value
      }
      // 路徑, 資料
      this.$http.post(`${VITE_APP_URL}/v2/admin/signin`, userData)
        .then((res) => {
          const { token, expired } = res.data
          document.cookie = `token = ${token}; expires = ${expired};`
          this.$router.push('/admin/products')
        })
        .catch((err) => {
          alert(err, '帳密錯誤')
        })
    }
  },
  mounted () {

  }
}
</script>
