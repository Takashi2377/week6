<template>
      <div id="delProductModal" ref="dModalRef" class="modal fade" tabindex="-1" aria-labelledby="delProductModalLabel"
        aria-hidden="true">
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
              <strong class="text-danger">{{ item.title }}</strong> 商品(刪除後將無法恢復)。
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
import axios from 'axios'
import { onMounted, ref } from 'vue'
import { Modal } from 'bootstrap'

export default {
  props: ['item'],
  setup (props, { emit }) {
    const apiUrl = 'https://vue3-course-api.hexschool.io/v2'
    const apiPath = 'yuetin-hexschool'
    let delProductModal = null
    const dModalRef = ref(null)

    const delProduct = () => {
      axios
        .delete(`${apiUrl}/api/${apiPath}/admin/product/${props.item.id}`)
        .then(() => {
          emit('update')
          hideModal()
        })
        .catch((err) => {
          alert(err.response.data.message)
        })
    }

    const openModal = () => {
      delProductModal.show()
    }

    const hideModal = () => {
      delProductModal.hide()
    }

    onMounted(() => {
      delProductModal = new Modal(dModalRef.value, {
        keyboard: false,
        backdrop: 'static'
      })
    })

    return {
      dModalRef,
      delProduct,
      openModal
    }
  }
}
</script>
