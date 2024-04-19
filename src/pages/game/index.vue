<script lang="ts">
import { defineComponent } from 'vue';
import GameBoard from '@/widgets/game-board/index.vue';

export default defineComponent({
  name: 'game-page',
  components: { GameBoard },
  data() {
    return {
    };
  },
});
</script>

<template>
  <div>
    <game-board ref="board"/>
  </div>
</template>

<style scoped lang="scss">

</style>