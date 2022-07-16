<script setup>
import { ref } from 'vue'
import { useCounterStore } from '@/stores/counter.js'
const storeData = useCounterStore()
const parentMessage = ref('parentData')
const items = ref([{ message: 'rita' }, { message: 'nyein' }])
items.value.forEach((item,index) => {
  console.log(parentMessage, item.message, index)
});
</script>

<template>
  <div class="token_data">
    <h2>This is Token.</h2>
    {{ storeData.token }}
  </div>
  <div v-for="(item, index) in items" :key="index">
    {{ parentMessage }} - {{index}} - {{item.message}}
  </div>
</template>

<style scoped>
h2 {
  padding: 20px 0;
}
</style>