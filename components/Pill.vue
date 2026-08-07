<script setup lang="ts">
defineProps<{ spot?: boolean; onDark?: boolean }>()
</script>

<template>
  <span class="pill" :class="{ '-spot': spot, '-on-dark': onDark }">
    <slot />
  </span>
</template>
