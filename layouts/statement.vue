<template>
  <div class="slidev-layout -graphite statement-frame">
    <slot />
  </div>
</template>

<style scoped>
.statement-frame {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 1.2rem;
}
</style>
