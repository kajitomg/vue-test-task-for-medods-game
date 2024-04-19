<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'content-container',
});
</script>

<template>
  <div class="content-container">
    <div class="content-container__content">
      <slot></slot>
    </div>
  </div>
</template>

<style scoped lang="scss">

  .content-container {
    max-width: 1200px;
    justify-content: center;
    margin: 0 auto;
    padding: 16px;
    width: 100%;
    height: 100%;
  }

  .content-container__content {
    display: grid;
    grid-template-columns: 1fr;
    height: 100%;

  }
</style>