<template>
  <h2>前台頁面</h2>
  <nav>
  <RouterLink to="/">Home</RouterLink> |
  <RouterLink to="/about">About</RouterLink> |
  <RouterLink to="/products">產品列表</RouterLink> |
  <RouterLink to="/cart">購物車</RouterLink> |
  <RouterLink to="/login">登入</RouterLink> |
  <RouterLink to="/admin">後台</RouterLink>

  </nav>
  <router-view></router-view>
</template>
