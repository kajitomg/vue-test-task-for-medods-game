<script lang="ts">
import { defineComponent, PropType } from 'vue';

export interface SelectEvent extends Event {
  target: HTMLSelectElement;
}

/*
enum Modes {
  'all' = 'all',
  'music' = 'music',
  'highlight' = 'highlight',
}
*/

export type Mode = {
  name: string,
  value: 'all' | 'music' | 'highlight'
}

export const AllMode:Mode = {
  name: 'Музыка и подсветка',
  value: 'all',
};

export const MusicMode:Mode = {
  name: 'Музыка',
  value: 'music',
};

export const HighlightMode:Mode = {
  name: 'Подсветка',
  value: 'highlight',
};

export default defineComponent({
  name: 'game-options-mode',
  data() {
    return {
      options: {
        all: AllMode,
        music: MusicMode,
        highlight: HighlightMode,
      },
    };
  },
  props: {
    mode: {
      type: Object as PropType<Mode>,
      required: true,
    },
    setMode: {
      type: Function as PropType<(mode:Mode) => void>,
    },
  },
  methods: {
    onChange(event: SelectEvent) {
      const name = event.target.value as keyof typeof this.options;

      if (this.setMode) this.setMode(this.options?.[name]);
    },
  },
});
</script>

<template>
  <fieldset>
    <legend>Выберите режим:</legend>
    <form class="options" @change="onChange">
      <label class="option" :for="option.name" v-for="option in options" :key="option.name">
        <input
            type="radio"
            :id="option.name"
            name="options"
            :value="option.value"
            :checked="option.name === mode.name"
        />
        <span>{{option.name}}</span>
      </label>
    </form>
  </fieldset>
</template>

<style scoped lang="scss">
  .options{
    display: flex;
    flex-direction: column;
  }
  .option {
    cursor: pointer;

    display: flex;
    align-items: center;
    padding: 4px;

    & span {
      margin-left: 4px;
    }
  }
</style>