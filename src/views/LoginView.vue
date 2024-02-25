<template>
  <h2>登入頁面</h2>
  <RouterLink to="/">回到前台</RouterLink>
  <div class="container">
      <div class="row justify-content-center">
        <h1 class="h3 mb-3 font-weight-normal">
          請先登入
        </h1>
        <div class="col-8">
          <form class="form-signin" @submit.prevent="login">
            <div class="form-floating mb-3">
              <input type="email" class="form-control" v-model="user.username" id="floatingInput"
                placeholder="name@example.com" required autofocus>
              <label for="floatingInput">Email address</label>
            </div>
            <div class="form-floating">
              <input type="password" class="form-control" v-model="user.password" id="floatingPassword"
                placeholder="Password" required>
              <label for="floatingPassword">Password</label>
            </div>
            <button class="btn btn-lg btn-primary w-100 mt-3" type="submit">
              登入
            </button>
          </form>
        </div>
      </div>
      <p class="mt-5 mb-3 text-muted">
        &copy; 2023~∞ - 六角學院
      </p>
    </div>
</template>

<script>
import axios from 'axios'
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const { VITE_URL } = import.meta.env

export default {
  // data () {
  //   return {
  //     user: {
  //       username: '',
  //       password: ''
  //     }
  //   }
  // },
  // methods: {
  //   login () {
  //     axios.post(`${VITE_URL}/v2/admin/signin`, this.user).then((response) => {
  //       const { token, expired } = response.data
  //       // 寫入 cookie token
  //       // expires 設置有效時間
  //       document.cookie = `hexToken=${token};expires=${new Date(expired)}; path=/`
  //       this.$router.push('/admin/products')
  //     }).catch((err) => {
  //       alert(err.response.data.message)
  //     })
  //   }
  // }
  setup () {
    const user = ref({
      username: '',
      password: ''
    })
    const router = useRouter()
    const login = () => {
      axios.post(`${VITE_URL}/v2/admin/signin`, user.value).then((response) => {
        const { token, expired } = response.data
        // 寫入 cookie token
        // expires 設置有效時間
        document.cookie = `hexToken=${token};expires=${new Date(expired)}; path=/`
        router.push('/admin/products')
      }).catch((err) => {
        alert(err.response.data.message)
      })
    }

    return {
      user,
      login
    }
  }
}
</script>
