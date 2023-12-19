<script setup lang="ts">
import CpuVariable from './components/CpuVariable.vue';

const variables = ["ein"]
</script>

<template>
  <h1>Variables</h1>
  <CpuVariable v-for="cpuVar in variables" :key="cpuVar" :name="cpuVar" />
</template>