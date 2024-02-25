<template>
  <h2>產品管理</h2>
  <div class="container">
        <div class="text-end mt-4">
          <button class="btn btn-primary" @click="openModal('new')">
            建立新的產品
          </button>
        </div>
        <table class="table mt-4">
          <thead>
            <tr>
              <th width="120">分類</th>
              <th>產品名稱</th>
              <th width="120">原價</th>
              <th width="120">售價</th>
              <th width="100">是否啟用</th>
              <th width="120">編輯</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item) in products" :key="item.id">
              <td>{{ item.category }}</td>
              <td>{{ item.title }}</td>
              <td class="text">{{ item.origin_price }}</td>
              <td class="text">{{ item.price }}</td>
              <td>
                <span v-if="item.is_enabled" class="text-success">啟用</span>
                <span v-else>未啟用</span>
              </td>
              <td>
                <div class="btn-group">
                  <button
                    type="button"
                    class="btn btn-outline-primary btn-sm"
                    @click="openModal('edit', item)"
                  >
                    編輯
                  </button>
                  <button
                    type="button"
                    class="btn btn-outline-danger btn-sm"
                    @click="openModal('delete', item)"
                  >
                    刪除
                  </button>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
        <!-- 分頁元件 -->
        <pagination-component
          :pages="pagination"
          @emit-pages="getData"
        ></pagination-component>
        <!-- 分頁元件 -->
      </div>
      <!-- Modal -->
      <product-modal-component
        ref="pModal"
        :product="tempProduct"
        :is-new="isNew"
        @update="getData"
      ></product-modal-component>
      <del-product-modal-component
        ref="dModal"
        :item="tempProduct"
        @update="getData"
      ></del-product-modal-component>
</template>
<script>
import {
  ref,
  onMounted
} from 'vue'

import axios from 'axios'
import PaginationComponent from '../../components/PaginationComponent.vue'
import ProductModalComponent from '../../components/ProductModalComponent.vue'
import DelProductModalComponent from '../../components/DelProductModalComponent.vue'

const { VITE_URL, VITE_PATH } = import.meta.env

export default {
  components: {
    PaginationComponent,
    ProductModalComponent,
    DelProductModalComponent
  },
  setup () {
    const products = ref([])
    const tempProduct = ref({ imagesUrl: [] })
    const pagination = ref({})
    const isNew = ref(false)
    const pModal = ref(null)
    const dModal = ref(null)
    /* composition API 中，欲使用ref取得DOM元素，需先創建空的ref，
    名稱與模板中定義的ref屬性值相同，在onMounted階段後便會取得對應的DOM元素，記得return */

    const checkAdmin = () => {
      const url = `${VITE_URL}/v2/api/user/check`
      axios
        .post(url)
        .then(() => {
          getData()
        })
        .catch((err) => {
          alert(err.response.data.message)
          window.location = 'login.html'
        })
    }

    const getData = (page = 1) => {
      const url = `${VITE_URL}/v2/api/${VITE_PATH}/admin/products?page=${page}`

      axios
        .get(url)
        .then((response) => {
          products.value = response.data.products
          pagination.value = response.data.pagination
        })
        .catch((err) => {
          alert(err.response.data.message)
          window.location = 'login.html'
        })
    }

    const openModal = (type, item) => {
      if (type === 'new') {
        tempProduct.value = { imagesUrl: [] }
        isNew.value = true
        pModal.value.openModal() // 利用ref取得元件的DOM元素，從而呼叫元件中的方法
      } else if (type === 'edit') {
        tempProduct.value = { ...item }
        isNew.value = false
        pModal.value.openModal()
      } else if (type === 'delete') {
        tempProduct.value = { ...item }
        dModal.value.openModal()
      }
    }

    onMounted(() => {
      const token = document.cookie.replace(
        /(?:(?:^|.*;\s*)hexToken\s*=\s*([^;]*).*$)|^.*$/,
        '$1'
      )
      axios.defaults.headers.common.Authorization = token
      checkAdmin()
    })

    return {
      products,
      tempProduct,
      pagination,
      isNew,
      pModal, // 記得return
      dModal,
      getData,
      openModal
    }
  }
}
</script>
