<script lang="ts">
import './style.scss';
import AppScheme from '@/shared/components/app-scheme/index.vue';
import GamePage from '@/pages/game/index.vue';

export default {
  name: 'app',
  components: { GamePage, AppScheme },
};
</script>

<template>
  <app-scheme>
    <game-page/>
  </app-scheme>
</template>

<style lang="scss">

</style>