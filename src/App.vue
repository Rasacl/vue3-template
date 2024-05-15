<script setup>
import { RouterLink, RouterView } from 'vue-router'

const navList = [
  {
    name:'主页',
    path:'/'
  },
  {
    name:'测试',
    path:'/test'
  },
  {
    name:'关于',
    path:'/about'
  }
]
</script>

<template>
  <div class="swiper-box">
    <div class="nav-list">
       <div class="nav" v-for="(item,index) in navList" :key="index">{{ item.name }}</div>
    </div>
    <div class="view">
      <RouterView />
    </div>
  </div>
</template>

<style scoped lang="less">
.swiper-box{
  width: 100vw;
  height: 100vh;
  display: flex;
  .nav-list{
    width: 100px;
    height: 100%;
    background: #333;
  }
  .view{
    flex: 1;
  }
}
</style>
