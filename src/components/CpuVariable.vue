<script setup lang="ts">
import { ref } from 'vue'
import type { VarType } from '@/types/var-type.type'

defineProps<{ name: string, varType: VarType }>()

const varValue = ref<boolean | number>()

const save = () => {
  fetch("https://192.168.10.62/api/jsonrpc", {body: JSON.stringify({})})
}
</script>

<template>
  <div>
    <span class="right-distance">{{ name }}</span>
    <span class="right-distance">
      <span>Value:</span>
      <select v-if="varType === 'bool'" v-model="varValue">
        <option :value="false">False</option>
        <option :value="true">True</option>
      </select>
      <input type="number" v-else v-model="varValue">
    </span>
    <button @click="save">Save</button>
  </div>
</template>

<style scoped>
.right-distance {
  margin-right: 1rem;
}
</style>