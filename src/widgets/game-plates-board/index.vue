<script lang="ts">
import { defineComponent, PropType } from 'vue';
import GamePlate, { PlateRef } from '@/features/game-plate/index.vue';
import DO from '@/shared/assets/do.mp3';
import RE from '@/shared/assets/re.mp3';
import MI from '@/shared/assets/mi.mp3';
import FA from '@/shared/assets/fa.mp3';
import { Mode } from '@/features/game-options-mode/index.vue';
import { InfoType } from '@/pages/game/index.vue';

export type PlatesRefs = Record<string, PlateRef>

const DOaudio = new Audio(DO);
const REaudio = new Audio(RE);
const MIaudio = new Audio(MI);
const FAaudio = new Audio(FA);

export default defineComponent({
  name: 'game-plates-board',
  components: { GamePlate },
  props: {
    interval: {
      type: Number,
    },
    mode: {
      type: Object as PropType<Mode>,
      required: true,
    },
    info: {
      type: Object as PropType<InfoType>,
      required: true,
    },
    playbacked: {
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      refs: {} as PlatesRefs,
      sequence: [] as string[],
      audio: {
        do: DOaudio,
        re: REaudio,
        mi: MIaudio,
        fa: FAaudio,
      },
    };
  },
  watch: {
    interval(newVal) {
      this.setAudioSpeed(newVal);
    },
    info: {
      handler(newVal) {
        if (newVal.lose || newVal.win) {
          this.cleanSequence();
        }
      },
      deep: true,
    },
  },
  methods: {
    addToSequence(id: string) {
      if (!this.playbacked && !this.info.win && !this.info.lose) {
        this.sequence.push(id);
        this.$emit('sequence', this.sequence);
      }
    },
    cleanSequence() {
      this.sequence = [];
      this.$emit('sequence', []);
    },
    setAudioSpeed(speed: number) {
      const rate = 1000 / Math.min(speed, 1000);
      const keys = Object.keys(this.audio);

      for (let i = 0; i < keys.length; i += 1) {
        const key = keys[i] as keyof typeof this.audio;
        this.audio[key].playbackRate = rate;
      }
    },
  },
  mounted() {
    const refs = {
      blue: this.$refs.blue as PlateRef,
      red: this.$refs.red as PlateRef,
      yellow: this.$refs.yellow as PlateRef,
      green: this.$refs.green as PlateRef,
    };
    this.refs = refs;
    this.$emit('refs', refs);
  },
});
</script>

<template>
  <div class="board">
    <game-plate
        name="blue"
        class="board__plate"
        color="#1d75cd"
        ref="blue"
        :position="{top:true,left:true}"
        :audio="audio.do"
        :interval="interval"
        :mode="mode"
        :played="info.played"
        :playbacked="playbacked"
        :add-plate="addToSequence"
    />
    <game-plate
        name="red"
        class="board__plate"
        color="#8e0b0b"
        ref="red"
        :position="{top:true,right:true}"
        :audio="audio.re"
        :interval="interval"
        :mode="mode"
        :played="info.played"
        :playbacked="playbacked"
        :add-plate="addToSequence"
    />
    <game-plate
        name="yellow"
        class="board__plate"
        color="#dfdf11"
        ref="yellow"
        :position="{bottom:true,left:true}"
        :audio="audio.mi"
        :interval="interval"
        :mode="mode"
        :played="info.played"
        :playbacked="playbacked"
        :add-plate="addToSequence"
    />
    <game-plate
        name="green"
        class="board__plate"
        color="#30a40a"
        ref="green"
        :position="{bottom:true,right:true}"
        :audio="audio.fa"
        :interval="interval"
        :mode="mode"
        :played="info.played"
        :playbacked="playbacked"
        :add-plate="addToSequence"
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

    grid-template-rows: repeat(2, 1fr);

    grid-gap: 4px;
  }
  .board__plate {
    width: 100%;
    height: 100%;
  }
</style>