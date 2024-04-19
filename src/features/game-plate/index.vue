<script lang="ts">
import { defineComponent, PropType } from 'vue';
import {
  Mode,
  AllMode,
  MusicMode,
  HighlightMode,
} from '@/features/game-options-mode/index.vue';

const Component = defineComponent({
  name: 'game-plate',
  data() {
    return {
      highlighted: false,
      clicked: false,
      clickTime: 100,
    };
  },
  props: {
    name: {
      type: String,
      required: true,
    },
    audio: {
      type: Audio,
    },
    color: {
      type: String,
    },
    interval: {
      type: Number,
    },
    played: {
      type: Boolean,
    },
    mode: {
      type: Object as PropType<Mode>,
      required: true,
    },
    position: {
      type: Object as PropType<{
        top?:boolean,
        bottom?:boolean,
        left?:boolean,
        right?:boolean
      }>,
    },
    playbacked: {
      type: Boolean,
      required: true,
    },
    addPlate: {
      type: Function as PropType<(color:string) => void>,
    },
    setAudioSpeed: {
      type: Function as PropType<(speed:number) => void>,
    },
  },
  methods: {
    onClick() {
      if (!this.playbacked && this.played) {
        this.highlight(this.clickTime);
        this.playSong();
        this.click(this.clickTime);
        if (this.addPlate) this.addPlate(this.name);
      }
    },
    call() {
      this.highlight();
      this.playSong();
    },
    highlight(time?:number) {
      if (this.mode.value === AllMode.value || this.mode.value === HighlightMode.value) {
        this.highlighted = true;
        setTimeout(() => {
          this.highlighted = false;
        }, time || this.interval);
      }
    },
    click(time:number) {
      this.clicked = true;
      setTimeout(() => {
        this.clicked = false;
      }, time);
    },
    playSong() {
      if (this.mode.value === AllMode.value || this.mode.value === MusicMode.value) {
        this.audio?.play();
      }
    },
  },
});

export type PlateRef = InstanceType<typeof Component> | undefined

export default Component;
</script>

<template>
  <div
      class="plate"
      @click="onClick"
      @keydown="onClick"
      @keyup="onClick"
      :class="{
        highlighted,
        clicked,
        top:position?.top,
        bottom:position?.bottom,
        left:position?.left,
        right:position?.right,
      }"
      :style="{backgroundColor:color}"
  />
</template>

<style scoped lang="scss">
  .plate {
    cursor: pointer;

    width: 100px;
    height: 100px;

    transition: background-color 0.1s ease-in-out;

    &.highlighted {
      filter: brightness(200%) contrast(200%);
    }

    &.clicked {
      &.top {
        border-top: 3px #363636 solid;
      }

      &.bottom {
        border-bottom: 3px #363636 solid;
      }

      &.left {
        border-left: 3px #363636 solid;
      }

      &.right {
        border-right: 3px #363636 solid;
      }
    }
    &.top {
      &.left {
        border-radius: 100% 0 0 0;
      }
      &.right {
        border-radius: 0 100% 0 0;
      }
    }

    &.bottom {
      &.left {
        border-radius: 0 0 0 100%;
      }
      &.right {
        border-radius: 0 0 100% 0;
      }
    }
  }
</style>