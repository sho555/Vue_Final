<template>
    <router-link to="/">Home</router-link>|
    <router-link to="/about">About</router-link>|
    <router-link to="/products">Products</router-link>|
    <router-link to="/cart">Cart</router-link>|
    <router-link to="/admin">後台</router-link>|
    <router-link to="/login">登入</router-link>

    <hr>
    <RouterView></RouterView>
</template>

<script>
import { RouterView } from 'vue-router'

export default {
  components: {
    RouterView
  }
}
</script>

<style></style>
