<template>
  <nav class="navbar navbar-expand-lg navbar-light">
          <a class="navbar-brand" href="./index.html">G</a>
          <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavAltMarkup"
            aria-controls="navbarNavAltMarkup" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
          </button>
          <div class="collapse navbar-collapse justify-content-end" id="navbarNavAltMarkup">
            <div class="navbar-nav">

                <router-link to="/" class="nav-item nav-link me-4 active">
                  Home
                </router-link>
                <router-link to="/about" class="nav-item nav-link me-4">About</router-link>

                <router-link to="/products" class="nav-item nav-link me-4">Products</router-link>
                <router-link to="/cart" class="nav-item nav-link">
                  <i class="fas fa-shopping-cart"></i>
                </router-link>

            </div>
          </div>
        </nav>
    <!-- <router-link to="/admin">後台</router-link>|
    <router-link to="/login">登入</router-link> -->

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
