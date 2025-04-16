<!-- src/components/BaseDialog.vue -->
<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  show: {
    type: Boolean,
    default: false
  },
  title: {
    type: String,
    default: '提示'
  },
  componentOptions: {
    type: Array,
    default: () => [
      { label: '文本组件', value: 'text' },
      { label: '图片组件', value: 'image' }
    ]
  }
})

const selectedComponent = ref('')
const emit = defineEmits(['update:show', 'close'])

const localShow = ref(props.show)

watch(() => props.show, (newVal) => {
  localShow.value = newVal
})

watch(localShow, (newVal) => {
  if (!newVal) {
    emit('close')
    emit('update:show', false)
  }
})

function closeDialog() {
  localShow.value = false
}
</script>

<template>
  <transition name="fade">
    <div v-if="localShow" class="dialog-overlay" @click.self="closeDialog">
      <div class="dialog-container">
        <div class="dialog-header">
          <h3>{{ title }}</h3>
          <button class="close-btn" @click="closeDialog">×</button>
        </div>
        <div class="dialog-body">

          <!-- 添加组件选择区域 -->
          <div class="component-selector">
            <h4>选择要添加的组件类型:</h4>
            <div 
              v-for="option in componentOptions" 
              :key="option.value"
              class="option-item"
            >
              <input 
                type="radio" 
                :id="option.value" 
                :value="option.value" 
                v-model="selectedComponent"
              />
              <label :for="option.value">{{ option.label }}</label>
            </div>
          </div>

          <slot></slot>
        </div>
        <div class="dialog-footer">
          <slot name="footer">
            <button 
              class="confirm-btn" 
              @click="$emit('confirm', selectedComponent)"
              :disabled="!selectedComponent"
            >
              确定
            </button>
            <button class="confirm-btn" @click="closeDialog">关闭</button>
          </slot>
        </div>
      </div>
    </div>
  </transition>
</template>

<style scoped>
.dialog-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.dialog-container {
  background-color: white;
  border-radius: 8px;
  width: 400px;
  max-width: 90%;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.dialog-header {
  padding: 16px 20px;
  border-bottom: 1px solid #eee;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.dialog-header h3 {
  margin: 0;
  color: #333;
}

.close-btn {
  background: none;
  border: none;
  font-size: 20px;
  cursor: pointer;
  color: #999;
}

.close-btn:hover {
  color: #666;
}

.dialog-body {
  padding: 20px;
}

.dialog-footer {
  padding: 12px 20px;
  border-top: 1px solid #eee;
  display: flex;
  justify-content: flex-end;
}

.confirm-btn {
  padding: 8px 16px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.confirm-btn:hover {
  background-color: #3aa876;
}

/* 过渡动画 */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.fade-enter-active .dialog-container,
.fade-leave-active .dialog-container {
  transition: transform 0.3s;
}

.fade-enter-from .dialog-container,
.fade-leave-to .dialog-container {
  transform: scale(0.9);
}

/* 添加组件选择样式 */
.component-selector {
  margin-bottom: 20px;
}

.option-item {
  margin: 10px 0;
  display: flex;
  align-items: center;
}

.option-item input[type="radio"] {
  margin-right: 10px;
}

.option-item label {
  cursor: pointer;
}
</style>