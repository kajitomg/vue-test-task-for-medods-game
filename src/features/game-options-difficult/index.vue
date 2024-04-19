<script lang="ts">
import { defineComponent, PropType } from 'vue';

/*
enum DifficultyLevels {
  'easy' = 'easy',
  'medium' = 'medium',
  'difficult' = 'difficult',
}
*/

export interface SelectEvent extends Event {
  target: HTMLSelectElement;
}

export interface DifficultyLevel {
  name: string,
  value: 'easy' | 'medium' | 'difficult',
  interval: number, // ms
}

export const DifficultyLevelEasy:DifficultyLevel = {
  name: 'Легкий',
  value: 'easy',
  interval: 1500,
};
export const DifficultyLevelMedium:DifficultyLevel = {
  name: 'Средний',
  value: 'medium',
  interval: 1000,
};
export const DifficultyLevelDifficult:DifficultyLevel = {
  name: 'Сложный',
  value: 'difficult',
  interval: 400,
};

export default defineComponent({
  name: 'game-options-difficult',
  data() {
    return {
      options: {
        easy: DifficultyLevelEasy,
        medium: DifficultyLevelMedium,
        difficult: DifficultyLevelDifficult,
      },
    };
  },
  props: {
    difficult: {
      type: Object as PropType<DifficultyLevel>,
      required: true,
    },
    setDifficult: {
      type: Function as PropType<(difficult:DifficultyLevel) => void>,
    },
  },
  methods: {
    onChange(event: SelectEvent) {
      const name = event.target.value as keyof typeof this.options;

      if (this.setDifficult) this.setDifficult(this.options?.[name]);
    },
  },
});
</script>

<template>
  <fieldset>
    <legend>Выберите сложность:</legend>
    <form class="options" @change="onChange">
      <label class="option" :for="option.name" v-for="option in options" :key="option.name">
        <input
            type="radio"
            :id="option.name"
            name="options"
            :value="option.value"
            :checked="option.name === difficult.name"
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