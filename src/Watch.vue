<template>
    <input type="text" v-model="name">
    <p>Temps: {{ time }}</p>
    <button @click="reset">Renitialiser</button>
</template>

<script setup>
import { ref, watch, watchEffect } from 'vue'
import { useTimer } from './components/useTimer'

const {time, reset} = useTimer()
const name = ref('')

watch(name, (newValue, oldValue) => {
  document.title = newValue
}, { immediate: false })

watchEffect(() => {
  document.title = name.value
})
</script>