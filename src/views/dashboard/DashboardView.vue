<template>
  <h2>後台頁面</h2>
  <nav>
    <RouterLink to="/admin/order">訂單管理</RouterLink> |
  <RouterLink to="/admin/products">產品管理</RouterLink> |
  <RouterLink to="/">回到前台</RouterLink>
  </nav>
  <router-view></router-view>
</template>

<script>
import axios from 'axios'
import {
  onMounted
} from 'vue'

const { VITE_URL } = import.meta.env

export default {
  setup () {
    const checkAdmin = () => {
      const url = `${VITE_URL}/v2/api/user/check`
      axios
        .post(url)
        .then(() => {

        })
        .catch((err) => {
          alert(err.response.data.message)
        })
    }

    onMounted(() => {
      const token = document.cookie.replace(
        /(?:(?:^|.*;\s*)hexToken\s*=\s*([^;]*).*$)|^.*$/,
        '$1'
      )
      axios.defaults.headers.common.Authorization = token
      checkAdmin()
    })
  }
}
</script>
