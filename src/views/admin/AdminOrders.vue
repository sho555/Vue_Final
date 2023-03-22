<template>

    後臺訂單
    <div class="container">
          <table class="table mt-4">
            <thead>
              <tr>

                <th width="120">
                  購買項目
                </th>
                <th width="120">
                  信箱
                </th>
                <th width="120">購買時間</th>

                <th width="120">
                  應付金額
                </th>
                <th width="100">
                  是否付款
                </th>
                <th width="120">
                  編輯
                </th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, key) in orders" :key="key">
                <td>
                    <div v-for="(product, i) in item.products" :key="i">
                        {{ product.product.title }} 數量： {{ product.qty }} {{ product.product.unit }}
                    </div>
                </td>
                <td>{{ item.user.email }}</td>
                <td class="text-end">
                  {{ getDate(item.create_at) }}
                </td>
                <td class="text-end">
                    {{ item.total }}
                </td>
                <td>
                    <label class="form-check-label" for="is_paid">
                            <span style="color:red" v-if="item.is_paid">O</span>
                            <span style="color: green;" v-else>X</span>
                        </label>
                </td>
                <td>
                  <div class="btn-group">
                    <button type="button" class="btn btn-outline-primary btn-sm" @click="openModal('edit', order)">
                      編輯
                    </button>
                    <button type="button" class="btn btn-outline-danger btn-sm" @click="openModal('delete', order)">
                      刪除
                    </button>
                  </div>
                </td>
              </tr>
            </tbody>
          </table>
          <pagination :pages="page"
          :get-data="getData"></pagination>
        </div>
</template>

<script>
// import { Modal } from 'bootstrap'
// import Pagination from '../components/Pagination.vue'
// import AdminOrderModal from './AdminOrderModal.vue'
// import AdminOrderModalDelete from './AdminOrderModalDelete.vue'

const { VITE_APP_URL, VITE_APP_PATH } = import.meta.env

export default {
  data () {
    return {
      orders: {},
      isLoading: false,
      tempOrder: {},
      currentPage: 1
    }
  },
  methods: {
    getOrders () {
      const url = `${VITE_APP_URL}api/${VITE_APP_PATH}/admin/orders`
      this.isLoading = true
      this.$http.get(url).then((res) => {
        this.orders = res.data.orders
        this.isLoading = false
      }).catch((err) => {
        this.isLoading = false
        console.log(err)
      })
    }
  },
  mounted () {
    this.getOrders()
  }
}
</script>
