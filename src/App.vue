<script setup lang="ts">
import axios from 'axios'
import { onMounted, onUnmounted, ref } from 'vue'
import CpuVariable from '@/components/CpuVariable.vue'
import type { VarType } from '@/types/var-type.type'

const variables: { name: string; type: VarType }[] = [
  { name: '"dVisu".sinus', type: 'number' },
  { name: '"dVisu".cosinus', type: 'number' },
  { name: '"Motor".ein', type: 'bool' },
  { name: '"Motor".störungQuittieren', type: 'bool' },
  { name: '"Motor".drehrichtungInvertieren', type: 'bool' },
  { name: '"Motor".Sollgeschwindigkeit', type: 'number' },
  { name: '"Motor".Istgeschwindigkeit', type: 'number' },
  { name: '"Motor".freigegeben', type: 'bool' },
  { name: '"Motor".lokalBetrieb', type: 'bool' },
  { name: '"Motor".störung', type: 'bool' }
]

const displayedVars = ref<{ name: string; type: VarType; value: boolean | number }[]>([])

const token = ref<string | null>(null)

let interval: number | undefined

const getVars = async () => {
  const myVars = variables.map((myVar, index) => ({
    id: index,
    jsonrpc: '2.0',
    method: 'PlcProgram.Read',
    params: {
      var: myVar.name
    }
  }))

  const response = await axios.post('https://192.168.10.62/api/jsonrpc', myVars, {
    headers: { 'X-Auth-Token': token.value }
  })

  displayedVars.value = response.data.map((cpuVar: { id: number; result: boolean | number }) => ({
    name: variables[cpuVar.id].name,
    type: variables[cpuVar.id].type,
    value: cpuVar.result
  }))
}

onMounted(() => {
  axios
    .post('https://192.168.10.62/api/jsonrpc', {
      id: 1,
      jsonrpc: '2.0',
      method: 'Api.Login',
      params: {
        user: '5AHIT',
        password: '5ahiT'
      }
    })
    .then((response) => {
      token.value = response.data.result.token
      getVars()
    })
  interval = setInterval(getVars, 1000)
})

onUnmounted(() => clearInterval(interval))
</script>

<template>
  <div class="table list">
    <h1>Variables</h1>
    <CpuVariable
      v-for="cpuVar in displayedVars"
      :key="cpuVar.name"
      :name="cpuVar.name"
      :var-type="cpuVar.type"
      :token="token"
      :value="cpuVar.value"
    />
  </div>
</template>

<style scoped>
.list {
  width: 80%;
  margin-left: auto;
  margin-right: auto;
}
</style>
