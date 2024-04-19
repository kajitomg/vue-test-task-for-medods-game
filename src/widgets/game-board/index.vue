<script lang="ts">
import { defineComponent, PropType } from 'vue';
import GamePlatesBoard, { PlatesRefs } from '@/widgets/game-plates-board/index.vue';
import { InfoType, SettingsType } from '@/pages/game/index.vue';

const Component = defineComponent({
  name: 'game-board',
  components: { GamePlatesBoard },
  props: {
    settings: {
      type: Object as PropType<SettingsType>,
      required: true,
    },
    info: {
      type: Object as PropType<InfoType>,
      required: true,
    },
    level: {
      type: [] as PropType<string[]>,
      required: true,
    },
    setWin: {
      type: Function as PropType<(value: boolean) => void>,
      required: true,
    },
    setLose: {
      type: Function as PropType<(value: boolean) => void>,
      required: true,
    },
    setPlayed: {
      type: Function as PropType<(value: boolean) => void>,
      required: true,
    },
  },
  data() {
    return {
      refs: {} as PlatesRefs,
      sequence: [] as string[],
    };
  },
  watch: {
    isLose(newValue) {
      this.setLose(newValue);
    },
    isWin(newValue) {
      this.setWin(newValue);
    },
  },
  computed: {
    isLose():boolean {
      if (!this.info.lose) {
        return !this.compareSequences(this.sequence);
      }
      return this.info.lose;
    },
    isWin():boolean {
      if (!this.info.win) {
        return this.compareSequences(this.sequence)
            && this.sequence.length === this.level.length;
      }
      return this.info.win;
    },
  },
  methods: {
    callPlate(value:string) {
      this.refs?.[value]?.call();
    },
    initRefs(refs: PlatesRefs) {
      this.refs = refs;
      this.$emit('refs', refs);
    },
    updateSequence(sequence:string[]) {
      this.sequence = sequence;
    },
    compareSequences(sequence:string[]):boolean {
      const levelSequence = [...this.level]
        .splice(0, sequence.length);
      console.log(sequence, this.level);
      return sequence.toString() === levelSequence.toString();
    },
  },
});

export type BoardRef = InstanceType<typeof Component> | undefined

export default Component;
</script>

<template>
  <div>
    <game-plates-board
        v-on:refs="initRefs"
        :interval="settings.difficulty.interval"
        :mode="settings.mode"
        :info="info"
        :playbacked="info.playbacked"
        v-on:sequence="updateSequence"
    />
  </div>
</template>

<style scoped lang="scss">

</style>