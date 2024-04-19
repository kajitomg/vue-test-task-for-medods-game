<script lang="ts">
import { defineComponent } from 'vue';
import ContentContainer from '@/shared/ui/content-container/index.vue';

export default defineComponent({
  name: 'app-scheme',
  components: { ContentContainer },
});
</script>

<template>
  <div class="app__layout">
    <main class="main">
      <content-container>
        <div class="main__content">
          <slot></slot>
        </div>
      </content-container>
    </main>
  </div>
</template>

<style scoped lang="scss">
  .app__layout {
    position: relative;

    display: flex;
    flex-direction: column;
  }

  .main {
    display: flex;
    flex-direction: column;

    flex: 1 0 100vh;
  }

  .main__content {
    height: 100%;
  }
</style>