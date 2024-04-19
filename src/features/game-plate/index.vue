<script lang="ts">
import { Component, defineComponent, PropType } from 'vue';

const component = defineComponent({
  name: 'game-plate',
  data() {
    return {
      highlighted: false,
    };
  },
  props: {
    audio: {
      type: Audio,
    },
    color: {
      type: String,
    },
    time: {
      type: Number,
    },
    position: {
      type: Object as PropType<{
        top?:boolean,
        bottom?:boolean,
        left?:boolean,
        right?:boolean
      }>,
    },
  },
  methods: {
    call() {
      this.highlight();
      this.playSong();
    },
    highlight() {
      this.highlighted = true;
      setTimeout(() => {
        this.highlighted = false;
      }, this.time);
    },
    playSong() {
      this.audio?.play();
    },
  },
});

export type plateRef = Component<typeof component>

export default component;
</script>

<template>
  <div
      class="plate"
      :class="{
        highlighted,
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
    width: 100px;
    height: 100px;

    transition: 0.1s ease-in-out;

    &.highlighted {
      filter: brightness(200%) contrast(200%);
      &.top {
        border-top: 1px #363636 solid;
      }

      &.bottom {
        border-bottom: 1px #363636 solid;
      }

      &.left {
        border-left: 1px #363636 solid;
      }

      &.right {
        border-right: 1px #363636 solid;
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