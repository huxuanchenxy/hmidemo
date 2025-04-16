<!-- src/components/MyCustomComponent.vue -->
<script setup>
import { ref } from 'vue'
import BaseDialog from './BaseDialog.vue'
import { inject } from 'vue'
import { onMounted } from 'vue'
import { shallowRef, defineAsyncComponent } from 'vue'


// 动态导入组件
const components = {
  text: defineAsyncComponent(() => import('./TextComponent.vue')),
  image: defineAsyncComponent(() => import('./ImageComponent.vue'))
}
const title = ref('动态组件加载演示')

const currentComponent = shallowRef(null)
const componentType = ref('')

// 确认选择
function handleConfirm(selectedType) {
  componentType.value = selectedType
  currentComponent.value = components[selectedType]
  showDialog.value = false
}

const setToken = inject('setToken')
onMounted(() => {
  setToken(generateToken())
})

// const title = ref('这是我的自定义组件')
const items = ref([
  { id: 1, name: '项目一' },
  { id: 2, name: '项目二' },
  { id: 3, name: '项目三' }
])

const showDialog = ref(false)
const selectedItem = ref(null)

function handleClick(item) {
  selectedItem.value = item
  showDialog.value = true
}

// 模拟获取token的函数
function generateToken() {
  return `token_${Math.random().toString(36).substring(2, 10)}_${Date.now()}`
}

// 打开对话框
function openComponentDialog() {
  showDialog.value = true
}
</script>

<template>
  <div class="custom-component">
    <h2>{{ title }}</h2>
    <!-- <ul>
      <li 
        v-for="item in items" 
        :key="item.id"
        @click="handleClick(item)"
      >
        {{ item.name }}
      </li>
    </ul>
    <p>这是一个示例组件，展示了基本功能</p> -->

    <!-- 添加 Dialog 组件 -->
    <!-- <BaseDialog 
      v-model:show="showDialog" 
      :title="selectedItem ? `选择第三方组件 - ${selectedItem.name}` : '选择第三方组件'"
    >
      <div v-if="selectedItem">
        <p>你选择了: <strong>{{ selectedItem.name }}</strong></p>
        <p>项目ID: {{ selectedItem.id }}</p>
      </div>

      <template #footer>
        <button class="close-btn" @click="showDialog = false">关闭</button>
      </template>
    </BaseDialog> -->



    <button @click="openComponentDialog">选择要加载的组件</button>
    
    <!-- 动态组件渲染区域 -->
    <div class="component-container">
      <component 
        :is="currentComponent" 
        v-if="currentComponent"
      />
      <p v-else class="placeholder">请选择要加载的组件</p>
    </div>

        <!-- 对话框 -->
        <BaseDialog 
      v-model:show="showDialog" 
      title="选择组件类型"
      @confirm="handleConfirm"
    />
  </div>
</template>

<style scoped>
.custom-component {
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
  margin-top: 20px;
  background-color: #f9f9f9;
}

.custom-component h2 {
  color: #42b983;
  margin-bottom: 15px;
}

.custom-component ul {
  list-style: none;
  padding: 0;
}

.custom-component li {
  padding: 8px 12px;
  margin: 5px 0;
  background-color: white;
  border: 1px solid #eee;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.custom-component li:hover {
  background-color: #f0f0f0;
}

/* 对话框按钮样式 */
.close-btn {
  padding: 8px 16px;
  background-color: #f0f0f0;
  color: #333;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-left: 10px;
}

.close-btn:hover {
  background-color: #e0e0e0;
}


.component-container {
  min-height: 200px;
  padding: 15px;
  border: 1px dashed #ccc;
  border-radius: 4px;
}
</style>