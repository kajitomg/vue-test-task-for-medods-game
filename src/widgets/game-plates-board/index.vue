<script lang="ts">
import { defineComponent, PropType } from 'vue';
import GamePlate, { plateRef } from '@/features/game-plate/index.vue';
import DO from '@/shared/assets/do.mp3';
import RE from '@/shared/assets/re.mp3';
import MI from '@/shared/assets/mi.mp3';
import FA from '@/shared/assets/fa.mp3';

const DOaudio = new Audio(DO);
const REaudio = new Audio(RE);
const MIaudio = new Audio(MI);
const FAaudio = new Audio(FA);

export default defineComponent({
  name: 'game-plates-board',
  components: { GamePlate },
  data() {
    return {
      refs: Object as PropType<{ [name:string]:plateRef }>,
      doAudio: DOaudio,
      reAudio: REaudio,
      miAudio: MIaudio,
      faAudio: FAaudio,
    };
  },
  mounted() {
    this.refs = {
      blue: this.$refs.blue,
      red: this.$refs.red,
      yellow: this.$refs.yellow,
      green: this.$refs.green,
    };
  },
});
</script>

<template>
  <div class="board">
    <game-plate
        class="board__plate"
        color="blue"
        ref="blue"
        :position="{top:true,left:true}"
        :audio="doAudio"
    />
    <game-plate
        class="board__plate"
        color="red"
        ref="red"
        :position="{top:true,right:true}"
        :audio="reAudio"
    />
    <game-plate
        class="board__plate"
        color="yellow"
        ref="yellow"
        :position="{bottom:true,left:true}"
        :audio="miAudio"
    />
    <game-plate
        class="board__plate"
        color="green"
        ref="green"
        :position="{bottom:true,right:true}"
        :audio="faAudio"
    />
  </div>
</template>

<style scoped lang="scss">
  .board {
    width: 200px;
    height: 200px;
    overflow: hidden;

    border-radius: 50%;
    border: 5px whitesmoke solid;

    box-shadow: 0 0 10px 5px rgba(0, 0, 0, 0.25);

    display: grid;

    grid-template-columns: repeat(2, 1fr);
  }
  .board__plate {
    width: 100%;
    height: 100%;
  }
</style>