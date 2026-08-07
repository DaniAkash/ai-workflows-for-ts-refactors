<template>
  <div class="slidev-layout cover-frame">
    <slot />
  </div>
</template>

<style scoped>
.cover-frame {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 1.4rem;
}
</style>
