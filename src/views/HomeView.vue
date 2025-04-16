<script setup>
import { ref } from 'vue'
import { useCounterStore } from '../stores/counter.js'
import MyCustomComponent from '../components/MyCustomComponent.vue' // 导入自定义组件
import { provide } from 'vue'
provide('setToken', (token) => {
  receivedToken.value = token
})

const counter = useCounterStore()
const msg = ref('父页面画布!')
const receivedToken = ref(null)
</script>

<template>
  <div>
    <h1>{{ msg }}</h1>
        <!-- 添加自定义组件 -->
        <MyCustomComponent />

            <!-- 显示接收到的token -->
    <div v-if="receivedToken" class="token-display">
      <h3>从子组件接收的Token:</h3>
      <p>{{ receivedToken }}</p>
    </div>
  </div>
</template>

<style scoped>
h1 {
  color: #42b983;
}

.token-display {
  margin-top: 20px;
  padding: 15px;
  background-color: #f5f5f5;
  border-radius: 8px;
  border-left: 4px solid #42b983;
}

.token-display h3 {
  margin-top: 0;
  color: #42b983;
}

.token-display p {
  word-break: break-all;
  font-family: monospace;
}
</style>