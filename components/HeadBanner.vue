<template>
  <el-carousel :interval="3000"  arrow="always" :autoplay="true" :loop="true" height="450px">
    <el-carousel-item v-for="banner in banners" :key="banner">
      <el-image :src="banner" width="100%" height="450px">
      <div slot="placeholder" class="image-slot">
          <img class="loddingImg" src="~/assets/images/loading.gif"  width="84" height="84b"/>
        </div>
      </el-image>
    </el-carousel-item>
  </el-carousel>
</template>

<script>
  export default {
    name: 'head-banner',
    data(){
      return {
        banners:[require("~/static/images/banner-1.jpg")]
      }
    }
  }
</script>

<style scoped>
  .el-image,.el-image img{
    width: 100%; height: 450px

  }
</style>
