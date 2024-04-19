<script lang="ts">
import { defineComponent, PropType } from 'vue';
import GamePlate, { plateRef } from '@/features/game-plate/index.vue';
import DO from '@/shared/assets/do.mp3';
import RE from '@/shared/assets/re.mp3';
import MI from '@/shared/assets/mi.mp3';
import FA from '@/shared/assets/fa.mp3';
import GamePlatesBoard from '@/widgets/game-plates-board/index.vue';

const DOaudio = new Audio(DO);
const REaudio = new Audio(RE);
const MIaudio = new Audio(MI);
const FAaudio = new Audio(FA);

export default defineComponent({
  name: 'game-board',
  components: { GamePlatesBoard, GamePlate },
  data() {
    return {
      refs: Object as PropType<{ [name:string]:plateRef }>,
      sequence: [
        { ref: this.$parent?.$refs.red },
        { ref: this.$parent?.$refs.bule },
        { ref: this.$parent?.$refs.red },
      ],
      doAudio: DOaudio,
      reAudio: REaudio,
      miAudio: MIaudio,
      faAudio: FAaudio,
    };
  },
  methods: {
    play(
      callback:(value:string) => void,
      sequence:string[],
      interval = 1000,
      timeout = 1000,
    ) {
      return () => {
        setTimeout(() => {
          const iterator = this.makeGenerator(sequence);

          this.call(iterator, callback, interval);
        }, timeout);
      };
    },
    makeGenerator(sequence:string[]) {
      function* makeIterator() {
        for (let i = 0; i < sequence.length; i += 1) {
          if (i !== sequence.length - 1) yield sequence[i];
        }
        return sequence[sequence.length - 1];
      }
      return makeIterator();
    },
    call(
      iterated: Generator<string, string, unknown>,
      callback:(value:string) => void,
      interval:number,
    ) {
      const value = iterated.next();
      callback(value.value);
      setTimeout(() => {
        if (!value.done) this.call(iterated, callback, interval);
      }, interval);
    },
    callback(value:string) {
      this.refs?.[value]?.call();
    },
    initRefs(refs: PropType<{ [name:string]:plateRef }>) {
      this.refs = refs;
      this.$emit('refs', refs);
    },
  },
  props: {

  },
});
</script>

<template>
  <div>
    <button @click="(e) => play(callback,['red','blue','red'])(e)">Играть</button>
    <game-plate color="darkred" :time="700" ref="red" :audio="doAudio"/>
    <game-plate color="royalblue" :time="700" ref="blue" :audio="reAudio"/>
    <game-plates-board v-on:refs="initRefs"/>
  </div>
</template>

<style scoped lang="scss">

</style>