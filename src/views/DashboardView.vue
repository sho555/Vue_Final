<template>
    <router-link to="/admin/products">後臺產品列表</router-link>|
    <router-link to="/admin/orders">後臺訂單列表</router-link>|
    <router-link to="/home">回前台首頁</router-link>|
    <a href="#" @click.prevent="logout">登出</a>

    <hr>
    <RouterView></RouterView>

</template>

<script>
import { RouterView } from 'vue-router'
// const { VITE_APP_URL } = import.meta.env

export default {
  data () {
    return {
      products: [],
      product_detail: {},
      newProd: false,
      tempProduct: {
        imagesUrl: []
      },
      page: {}
    }
  },
  components: {
    RouterView
  },
  methods: {
    logout () {
      document.cookie = `token = ;expires = ${new Date()}` // 清除cookie
      this.$router.push('/login')
    },
    // checkLogin () {
    //   const token = document.cookie.replace(/(?:(?:^|.*;\s*)myToken\s*=\s*([^;]*).*$)|^.*$/, '$1') // 取出token
    //   this.$http.defaults.headers.common.Authorization = token // 將token驗證

    //   this.$http.post(`${VITE_APP_URL}/api/user/check`)
    //     .then(res => {
    //       if (!res.data.success) {
    //         this.$router.push('/login')
    //       }
    //     })
    //     .catch((err) => {
    //       console.log(err.response.data.message)
    //       this.$router.push('/login')
    //     })
    // },
    mounted () {
      // this.checkLogin()
    }
  }
}
</script>

<style></style>
