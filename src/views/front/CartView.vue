<!-- eslint-disable n/handle-callback-err -->
<template>
     <table class="table align-middle">
            <!-- <thead>
              <tr>
                <th>圖片</th>
                <th>商品名稱</th>
                <th>價格</th>
                <th></th>
              </tr>
            </thead> -->
            <tbody>
              <tr v-for="product in products" :key="product.id">
                <td style="width: 200px">
                  <div
                    style="
                      height: 100px;
                      background-size: cover;
                      background-position: center;
                    "
                    :style="{backgroundImage: `url(${product.imageUrl})`}"
                  ></div>
                </td>
                <td>{{ product.title }}</td>
                <td>
                  <div class="h5" v-if="product.price == product.origin_price">{{ product.price }} 元</div>
                  <div v-else>
                    <del class="h6">原價 {{ product.origin_price }} 元</del>
                    <div class="h5">現在只要 {{ product.price }} 元</div>
                  </div>
                </td>
                <td>
                  <div class="btn-group btn-group-sm">
                    <button type="button" class="btn btn-outline-secondary"
                      @click="openModal(product.id)">
                      <i class="fas fa-spinner fa-pulse"></i>
                      查看更多
                    </button>
                    <button type="button" class="btn btn-outline-danger"
                      @click="addToCart(product.id)">
                      <i class="fas fa-spinner fa-pulse"></i>
                      加到購物車
                    </button>
                  </div>
                </td>
              </tr>
            </tbody>
          </table>
          <div class="text-end">

          </div>
          <table class="table align-middle">
            <thead>
              <tr>
                <th></th>
                <th>品名</th>
                <th style="width: 150px">數量/單位</th>
                <th>單價</th>
                <button class="btn btn-outline-danger" type="button">
              清空購物車
            </button>
              </tr>
            </thead>
            <tbody>
              <template v-if="cart.carts">
                <tr v-for="item in cart.carts" :key="item.id">
                  <td>
                    <button type="button" class="btn btn-outline-danger btn-sm"
                      @click="deleteItem(item)"
                      :disabled="item.id === loadingItem"
                      >
                      <i class="fas fa-spinner fa-pulse"></i>
                      x
                    </button>
                  </td>
                  <td>
                    {{ item.product.title }}
                  </td>
                  <td>
                    <div class="input-group input-group-sm">
                      <select name="" id="" class="form-control" v-model="item.qty"
                        :disabled="item.id === loadingItem"
                        @change="updateCartItem(item)">
                        <option :value="i" v-for="i in 20" :key="i + '1234578'">{{i}}</option>
                      </select>
                    </div>
                  </td>
                  <td class="text-end">
                    {{ item.total }}
                  </td>
                </tr>
              </template>
            </tbody>
            <tfoot>
              <tr>
                <td colspan="3" class="text-end">總計</td>
                <td class="text-end">{{ cart.total }}</td>
              </tr>
              <tr>
                <td colspan="3" class="text-end text-success">折扣價</td>
                <td class="text-end text-success">{{ cart.final_total }}</td>
              </tr>
            </tfoot>
          </table>
</template>
<script>
const { VITE_APP_URL, VITE_APP_PATH } = import.meta.env

export default {
  data () {
    return {
      products: [],
      productId: '',
      cart: {},
      loadingItem: '' // 存id

    }
  },
  methods: {

    getCarts () {
      const url = `${VITE_APP_URL}/v2/api/${VITE_APP_PATH}/cart`
      this.$http.get(url)
        .then((res) => {
          this.cart = res.data.data
          console.log('購物車', res.data)
        })
        .catch((err) => {
          console.log(err.res.data.message)
          console.log("can't get carts Data")
        })
    },
    updateCartItem (item) { // 購物車的id, 產品的id
      // const url = `${VITE_APP_URL}/v2/api/${VITE_APP_PATH}/cart/${item.id}`;
      const data = {
        product_id: item.product.id,
        qty: item.qty
      }
      this.loadingItem = item.id
      this.$http.put(`${VITE_APP_URL}/v2/api/${VITE_APP_PATH}/cart/${item.id}`, {
        data
      })
        .then((res) => {
          console.log('更新購物車', res.data)
          this.getCarts()
          this.loadingItem = ''
        })
        .catch((err) => {
          console.log(err.res.data.message)

          console.log("can't get update Data")
        })
    },
    deleteItem (item) { // 購物車的id, 產品的id
      const url = `${VITE_APP_URL}/v2/api/${VITE_APP_PATH}/cart/${item.id}`
      this.loadingItem = item.id
      this.$http.delete(url)
        .then((res) => {
          console.log('刪除購物車', res.data)

          this.loadingItem = ''
          this.getCarts()
        })
        .catch((err) => {
          console.log(err.res.data.message)

          console.log("can't get delete Data")
        })
    }
  },
  mounted () {
    this.getCarts()
  }
}
</script>
