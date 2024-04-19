<script lang="ts">
import { defineComponent, PropType } from 'vue';
import GameOptionsDifficult, { DifficultyLevel } from '@/features/game-options-difficult/index.vue';
import GameOptionsMode, { Mode } from '@/features/game-options-mode/index.vue';

export default defineComponent({
  name: 'game-options',
  components: { GameOptionsMode, GameOptionsDifficult },
  props: {
    difficult: {
      type: Object as PropType<DifficultyLevel>,
      required: true,
    },
    setDifficult: {
      type: Function as PropType<(difficult:DifficultyLevel) => void>,
    },
    mode: {
      type: Object as PropType<Mode>,
      required: true,
    },
    setMode: {
      type: Function as PropType<(mode:Mode) => void>,
    },
  },
});
</script>

<template>
  <div class="options">
    <game-options-difficult :difficult="difficult" :set-difficult="setDifficult"/>
    <game-options-mode :mode="mode" :set-mode="setMode"/>
  </div>
</template>

<style scoped lang="scss">
  .options {
    padding: 16px;
  }
</style>