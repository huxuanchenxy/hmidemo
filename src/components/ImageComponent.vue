<!-- src/components/ImageComponent.vue -->
<script setup>
import { ref } from 'vue'
// const imageUrl = ref('https://via.placeholder.com/300x200')
const imageUrl = ref('https://ck-bkt-knowledge-payment.oss-cn-hangzhou.aliyuncs.com/admin/material/25_material_admin/image/img/dy_pc_index.png')
</script>

<template>
  <div class="image-component">
    <h3>图片组件</h3>
    <img :src="imageUrl" alt="动态图片" style="max-width: 100%;" />
    <input v-model="imageUrl" placeholder="输入图片URL" />
  </div>
</template>

<style scoped>
.image-component {
  padding: 15px;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin: 10px 0;
}
.image-component input {
  width: 100%;
  padding: 8px;
  margin-top: 10px;
}
</style>