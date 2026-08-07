<script setup lang="ts">
defineProps<{ items: { n: string; l: string }[] }>()
</script>

<template>
  <div class="statcard">
    <div v-for="(it, i) in items" :key="i" class="statcard-col">
      <div class="statcard-n mono">{{ it.n }}</div>
      <div class="statcard-l mono">{{ it.l }}</div>
    </div>
  </div>
</template>

<style scoped>
.statcard {
  display: grid;
  grid-auto-flow: column;
  grid-auto-columns: 1fr;
  background: var(--color-paper-white);
  border: 1px solid #000;
  border-radius: 12px;
  box-shadow: 5px 5px 0 0 #000;
  overflow: hidden;
}
.statcard-col {
  padding: 1.5rem 1.6rem;
  border-left: 1px solid #000;
}
.statcard-col:first-child {
  border-left: none;
}
.statcard-n {
  color: var(--color-signal-orange);
  font-weight: 700;
  font-size: 2.9rem;
  line-height: 1;
  letter-spacing: -0.02em;
}
.statcard-l {
  margin-top: 0.6rem;
  text-transform: uppercase;
  letter-spacing: 0.06em;
  font-size: 0.72rem;
  color: var(--color-carbon-black);
}
</style>
