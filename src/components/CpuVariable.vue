<script setup lang="ts">
defineProps<{ name: string }>()
</script>

<template>
  <div>
    <span>{{ name }}</span>
  </div>
</template>