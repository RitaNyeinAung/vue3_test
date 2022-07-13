<script setup>
import { useCounterStore } from '@/stores/counter.js'
const storeData = useCounterStore()
</script>

<template>
  <div class="token_data">
    <h2>This is Token.</h2>
    {{ storeData.token }}
  </div>
</template>

<style scoped>
h2 {
  padding: 20px 0;
}
</style>