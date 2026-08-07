<template>
  <div class="slidev-layout -carbon section-frame">
    <slot />
  </div>
</template>

<style scoped>
.section-frame {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 1rem;
}
</style>
