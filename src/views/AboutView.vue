<script setup>
import { ref } from 'vue'
import { useCounterStore } from '../stores/counter.js'
import HelloWorld from '../components/HelloWorld.vue'

const counter = useCounterStore()
const msg = ref('Hello Vue 3!')
</script>

<template>
  <div>
    <h1>{{ msg }}</h1>
    <HelloWorld />
    <p>Count: {{ counter.count }}</p>
    <button @click="counter.increment()">Increment</button>
  </div>
</template>

<style scoped>
h1 {
  color: #c528b8;
}
</style>