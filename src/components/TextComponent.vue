<!-- src/components/TextComponent.vue -->
<script setup>
import { ref } from 'vue'
const text = ref('这是动态加载的文本组件')
</script>

<template>
  <div class="text-component">
    <h3>文本组件</h3>
    <p>{{ text }}</p>
    <input v-model="text" placeholder="输入文本内容" />
  </div>
</template>

<style scoped>
.text-component {
  padding: 15px;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin: 10px 0;
}
.text-component input {
  width: 100%;
  padding: 8px;
  margin-top: 10px;
}
</style>