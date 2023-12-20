<script setup lang="ts">
import CpuVariable from '@/components/CpuVariable.vue';
import type { VarType } from '@/types/var-type.type'

const variables: [{ name: string, type: VarType }] = [{ name: "ein", type: "bool" }]
</script>

<template>
  <h1>Variables</h1>
  <CpuVariable v-for="cpuVar in variables" :key="cpuVar.name" :name="cpuVar.name" :var-type="cpuVar.type" />
</template>