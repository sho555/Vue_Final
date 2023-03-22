<template>

    後臺產品

        <div class="container">
          <div class="text-end mt-4">
            <button class="btn btn-primary" @click="openModal('new')">
              建立新的產品
            </button>
          </div>
          <table class="table mt-4">
            <thead>
              <tr>
                <th width="120">
                  分類
                </th>
                <th>產品名稱</th>
                <th width="120">
                  原價
                </th>
                <th width="120">
                  售價
                </th>
                <th width="100">
                  是否啟用
                </th>
                <th width="120">
                  編輯
                </th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="product in products" :key="product.id">
                <td>{{ product.category }}</td>
                <td>{{ product.title }}</td>
                <td class="text-end">
                  {{ product.origin_price }}
                </td>
                <td class="text-end">
                  {{ product.price }}
                </td>
                <td>
                  <span v-if="product.is_enabled" class="text-success">啟用</span>
                  <span v-else>未啟用</span>
                </td>
                <td>
                  <div class="btn-group">
                    <button type="button" class="btn btn-outline-primary btn-sm" @click="openModal('edit', product)">
                      編輯
                    </button>
                    <button type="button" class="btn btn-outline-danger btn-sm" @click="openModal('delete', product)">
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
        <div id="productModal" ref="productModal" class="modal fade" tabindex="-1" aria-labelledby="productModalLabel"
          aria-hidden="true">
          <div class="modal-dialog modal-xl">
            <div class="modal-content border-0">
              <div class="modal-header bg-dark text-white">
                <h5 id="productModalLabel" class="modal-title">
                  <span v-if="newProd">新增產品</span>
                  <span v-else>編輯產品</span>
                </h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
              </div>
              <div class="modal-body">
                <div class="row">
                  <div class="col-sm-4">
                    <div class="mb-3">
                      <label for="imageUrl" class="form-label">主要圖片</label>
                      <input v-model = "tempProduct.imageUrl" type="text" class="form-control mb-2" placeholder="請輸入圖片連結">
                      <img class="img-fluid" :src="tempProduct.imageUrl">
                    </div>
                    <h3 class="mb-3">多圖新增</h3>
                    <div v-if="Array.isArray(tempProduct.imagesUrl)">
                      <div class="mb-1" v-for="(image, key) in tempProduct.imagesUrl" :key="key">
                        <div class="mb-3">
                          <label for="imageUrl" class="form-label">圖片網址</label>
                          <input v-model="tempProduct.imagesUrl[key]" type="text" class="form-control"
                            placeholder="請輸入圖片連結">
                        </div>
                        <img class="img-fluid" :src="image">
                      </div>
                      <div v-if="!tempProduct.imagesUrl.length || tempProduct.imagesUrl[tempProduct.imagesUrl.length - 1]">
                        <button class="btn btn-outline-primary btn-sm d-block w-100"
                          @click="tempProduct.imagesUrl.push('')">
                          新增圖片
                        </button>
                      </div>
                      <div v-else>
                        <button class="btn btn-outline-danger btn-sm d-block w-100" @click="tempProduct.imagesUrl.pop()">
                          刪除圖片
                        </button>
                      </div>
                    </div>
                    <div v-else>
                      <button class="btn btn-outline-primary btn-sm d-block w-100" @click="createImages">
                        新增圖片
                      </button>
                    </div>
                  </div>
                  <div class="col-sm-8">
                    <div class="mb-3">
                      <label for="title" class="form-label">標題</label>
                      <input id="title" v-model="tempProduct.title" type="text" class="form-control" placeholder="請輸入標題">
                    </div>

                    <div class="row">
                      <div class="mb-3 col-md-6">
                        <label for="category" class="form-label">分類</label>
                        <input id="category" v-model="tempProduct.category" type="text" class="form-control"
                          placeholder="請輸入分類">
                      </div>
                      <div class="mb-3 col-md-6">
                        <label for="price" class="form-label">單位</label>
                        <input id="unit" v-model="tempProduct.unit" type="text" class="form-control" placeholder="請輸入單位">
                      </div>
                    </div>

                    <div class="row">
                      <div class="mb-3 col-md-6">
                        <label for="origin_price" class="form-label">原價</label>
                        <input id="origin_price" v-model.number="tempProduct.origin_price" type="number" min="0"
                          class="form-control" placeholder="請輸入原價">
                      </div>
                      <div class="mb-3 col-md-6">
                        <label for="price" class="form-label">售價</label>
                        <input id="price" v-model.number="tempProduct.price" type="number" min="0" class="form-control"
                          placeholder="請輸入售價">
                      </div>
                    </div>
                    <hr>

                    <div class="mb-3">
                      <label for="description" class="form-label">產品描述</label>
                      <textarea id="description" v-model = "tempProduct.description" type="text" class="form-control"
                        placeholder="請輸入產品描述">
                      </textarea>
                    </div>
                    <div class="mb-3">
                      <label for="content" class="form-label">說明內容</label>
                      <textarea id="description" v-model = "tempProduct.content" type="text" class="form-control"
                        placeholder="請輸入說明內容">
                      </textarea>
                    </div>
                    <div class="mb-3">
                      <div class="form-check">
                        <input id="is_enabled" v-model = "tempProduct.is_enabled" class="form-check-input" type="checkbox"
                          :true-value="1" :false-value="0">
                        <label class="form-check-label" for="is_enabled">是否啟用</label>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
              <div class="modal-footer">
                <button type="button" class="btn btn-outline-secondary" data-bs-dismiss="modal">
                  取消
                </button>
                <button type="button" class="btn btn-primary" @click="updateProducts">
                  確認
                </button>
              </div>
            </div>
          </div>
        </div>
        <div id="delProductModal" ref="delProductModal" class="modal fade" tabindex="-1"
          aria-labelledby="delProductModalLabel" aria-hidden="true">
          <div class="modal-dialog">
            <div class="modal-content border-0">
              <div class="modal-header bg-danger text-white">
                <h5 id="delProductModalLabel" class="modal-title">
                  <span>刪除產品</span>
                </h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
              </div>
              <div class="modal-body">
                是否刪除
                <strong class="text-danger">{{ tempProduct.title }}</strong> 商品(刪除後將無法恢復)。
              </div>
              <div class="modal-footer">
                <button type="button" class="btn btn-outline-secondary" data-bs-dismiss="modal">
                  取消
                </button>
                <button type="button" class="btn btn-danger" @click="delProduct">
                  確認刪除
                </button>
              </div>
            </div>
          </div>
        </div>

</template>

<script>
import Pagination from '../../components/PaginationView.vue'
const { VITE_APP_URL, VITE_APP_PATH } = import.meta.env
const productModal = {}
const delProductModal = {}

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
    Pagination
  },
  methods: {
    logout () {
      document.cookie = `token = ;expires = ${new Date()}` // 清除cookie
      this.$router.push('/login')
    },
    getData (page = 1) { // 取得產品資料 預設參數
      const url = `${VITE_APP_URL}/v2/api/${VITE_APP_PATH}/admin/products/?page= ${page}`
      this.$http.get(url)
        .then((res) => {
          this.products = res.data.products
          this.page = res.data.pagination
          console.log(this.page)
        })
        .catch((err) => {
          console.log(err.response.data.message)
        })
    },
    updateProducts () {
      let url = `${VITE_APP_URL}/api/${VITE_APP_PATH}/admin/product`
      let http = 'post'

      if (!this.newProd) {
        url = `${VITE_APP_URL}/api/${VITE_APP_PATH}/admin/product/${this.tempProduct.id}`
        http = 'put'
      }

      this.$http[http](url, { data: this.tempProduct }).then((res) => {
        alert(res.data.message)
        productModal.hide()
        this.getData()
      }).catch((err) => {
        console.log(err.response.data.message)
      })
    },
    openModal (newProd, product) {
      if (newProd === 'new') {
        this.tempProduct = {
          imagesUrl: []
        }
        this.newProd = true
        productModal.show()
      } else if (newProd === 'edit') {
        this.tempProduct = { ...product }
        this.newProd = false
        productModal.show()
      } else if (newProd === 'delete') {
        this.tempProduct = { ...product }
        delProductModal.show()
      }
    },
    delProduct () {
      const url = `${VITE_APP_URL}/api/${VITE_APP_PATH}/admin/product/${this.tempProduct.id}`

      this.$http.delete(url).then((res) => {
        alert(res.data.message)
        delProductModal.hide()
        this.getData()
      }).catch((err) => {
        console.log(err.response.data.message)
      })
    },
    createImages () {
      this.tempProduct.imagesUrl = []
      this.tempProduct.imagesUrl.push('')
    }
  },

  mounted () {
    this.getData()
    // this.checkLogin()
  }
}
</script>
