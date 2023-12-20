<script setup lang="ts">
import { ref } from 'vue'
import axios from 'axios'
import type { VarType } from '@/types/var-type.type'

const props = defineProps<{
  name: string
  varType: VarType
  token: string | null
  value: boolean | number
}>()

const newValue = ref<boolean | number>(props.varType === 'bool' ? false : 0.0)

const save = async () => {
  if (!props.token) {
    return
  }
  await axios.post(
    'https://192.168.10.62/api/jsonrpc',
    {
      id: 1,
      jsonrpc: '2.0',
      method: 'PlcProgram.Write',
      params: {
        var: props.name,
        value: newValue.value
      }
    },
    { headers: { 'X-Auth-Token': props.token } }
  )
}
</script>

<template>
  <div class="row rounded mt-1 mb-0 p-0">
    <span class="right-distance col">{{ name }}</span>
    <span class="right-distance col">
      <span>Value:</span>
      <span class="right-distance">{{ props.value }}</span>
    </span>
    <select class="col" v-if="varType === 'bool'" v-model="newValue">
      <option :value="false">False</option>
      <option :value="true">True</option>
    </select>
    <input class="col" type="number" v-else v-model="newValue" value="0" />
    <button class="col" @click="save">Save</button>
  </div>
</template>

<style scoped>
.right-distance {
  margin-right: 1rem;
}
</style>
