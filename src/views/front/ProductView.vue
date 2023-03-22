<template>
    <h2>{{ product.title }}</h2>
    <img :src="product.imageUrl" width="200" alt="">
    <h3>{{ product.price  }}</h3>
    <h4>{{ product.description }} </h4>
    <p>{{ product.content }}</p>
</template>
<script>

const { VITE_APP_URL, VITE_APP_PATH } = import.meta.env

export default {
  data () {
    return {
      product: []
    }
  },
  methods: {
    getProduct () {
      // eslint-disable-next-line no-irregular-whitespace
      const { id } = this.$route.params // 取出id

      this.$http.get(`${VITE_APP_URL}/v2/api/${VITE_APP_PATH}/product/${id}`)
        .then((res) => {
          this.product = res.data.product
          console.log(res.data.product)

          console.log(this.$route.params)
        }).catch((err) => {
          console.log(err.res.data.message)
        })
    }
  },
  mounted () {
    this.getProduct()
  }
}
</script>
