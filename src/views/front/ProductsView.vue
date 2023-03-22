<!-- eslint-disable no-irregular-whitespace -->
<!-- eslint-disable no-irregular-whitespace -->
<template>
    產品頁面
    <table class="table">
        <tr v-for="product in products" :key="product.id">
            <td>{{ product.title }}</td>
            <td> <img :src="product.imageUrl" width="200" alt=""> </td>
            <td>
                <RouterLink :to="`/product/${product.id}`" class = "btn btn-outline-secondary">產品細節</RouterLink>
                <button type="button" class="btn btn-outline-primary" @click="addCart(product.id)">加入購物車</button>
            </td>
        </tr>
    </table>
</template>

<script>
import { RouterLink } from 'vue-router'
const { VITE_APP_URL, VITE_APP_PATH } = import.meta.env

export default {
  data () {
    return {
      products: []
    }
  },
  components: {
    RouterLink
  },
  methods: {
    getProducts () {
      // eslint-disable-next-line no-irregular-whitespace
      this.$http.get(`${VITE_APP_URL}/v2/api/${VITE_APP_PATH}/products`)
        .then((res) => {
          this.products = res.data.products
          console.log(res)
        }).catch((err) => {
          console.log(err.res.data.message)
        })
    },
    addCart (id) {
      const data = {
        product_id: id,
        qty: 1
      }
      this.$http.post(`${VITE_APP_URL}/v2/api/${VITE_APP_PATH}/cart`, { data }) // this為main.js內的axios
        .then((res) => {
        //   this.data = res.data.cart
          console.log(res, 'cart')
        })
    }
  },
  mounted () {
    this.getProducts()
  }
}
</script>
