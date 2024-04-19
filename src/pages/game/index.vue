<script lang="ts">
import { defineComponent } from 'vue';
import GameBoard, { BoardRef } from '@/widgets/game-board/index.vue';
import { DifficultyLevel, DifficultyLevelEasy } from '@/features/game-options-difficult/index.vue';
import { Mode, AllMode } from '@/features/game-options-mode/index.vue';
import GameOptions from '@/widgets/game-options/index.vue';

/*
enum Plates {
  'blue' = 'blue',
  'red' = 'red',
  'yellow' = 'yellow',
  'green' = 'green',
}
*/

export type SettingsType = {
  difficulty: DifficultyLevel,
  mode: Mode,
  timeout: number,
}

export type InfoType = {
  played: boolean,
  win: boolean,
  lose: boolean,
  playbacked: boolean,
}

export default defineComponent({
  name: 'game-page',
  components: { GameOptions, GameBoard },
  data() {
    return {
      settings: {
        difficulty: DifficultyLevelEasy,
        mode: AllMode,
        timeout: 200,
      } as SettingsType,
      level: ['blue'] as string[],
      info: {
        played: false,
        win: false,
        lose: false,
        playbacked: false,
      } as InfoType,
      stats: {
        count: 0 as number,
      },
      error: 'Вы проиграли',
    };
  },
  watch: {
    info: {
      handler(newValue) {
        if (newValue.win) {
          setTimeout(() => {
            this.play(
              this.callback,
              this.level,
              this.settings.difficulty.interval,
              this.settings.timeout,
            )();
          }, 1000);
        }
      },
      deep: true,
    },
  },
  methods: {
    play(
      callback:(value:string) => void,
      sequence:string[],
      interval = 1000,
      timeout = 1000,
    ) {
      return () => {
        this.setLose(false);
        this.setWin(false);
        this.setPlayed(true);
        this.info.playbacked = true;
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
        if (value.done) this.info.playbacked = false;
      }, interval);
    },
    callback(value:string) {
      const board = this.$refs.board as BoardRef;
      board?.callPlate(value);
    },
    setDifficult(difficulty:DifficultyLevel) {
      this.settings.difficulty = difficulty;
    },
    setMode(value:Mode) {
      this.settings.mode = value;
    },
    setWin(value: boolean) {
      this.info.win = value;
      if (value) {
        this.setPlayed(false);
        this.stats.count += 1;
        this.level.push(this.getRandomPlate());
      }
    },
    setLose(value: boolean) {
      this.info.lose = value;
      if (value) {
        this.setPlayed(false);
        this.stats.count = 0;
        this.level = ['blue'];
      }
    },
    setPlayed(value: boolean) {
      this.info.played = value;
    },
    getRandomPlate(itemsCount = 4) {
      const random = this.getRandomNumber(1, itemsCount);
      return ['blue', 'red', 'yellow', 'green'][random];
    },
    getRandomNumber(min:number, max:number) {
      return Math.round(Math.random() * (max - min) + min);
    },
  },
});
</script>

<template>
  <div class="game">
    <game-board
        ref="board"
        :level="level"
        :settings="settings"
        :info="info"
        :set-win="setWin"
        :set-lose="setLose"
        :set-played="setPlayed"
    />
    <div class="stats">
      <button @click="() => {
      play(
        callback,
        level,
        settings.difficulty.interval,
        settings.timeout,
       )()
    }">Играть</button>
      <div class="error" v-if="info.lose">{{error}}</div>
      <game-options
          :difficult="settings.difficulty"
          :set-difficult="setDifficult"
          :mode="settings.mode"
          :set-mode="setMode"
      />
      <div>Счет: {{stats.count}}</div>
    </div>
  </div>
</template>

<style scoped lang="scss">
  .game {
    width: 100%;
    height: 100%;
    display: flex;

    align-items: center;
    justify-content: center;
  }
  .stats {
    margin: 16px;
  }
  button {
    padding: 4px 8px;

    border: 1px solid black;
    border-radius: 12px;

    font-weight: 700;
  }
  .error {
    color: red;
  }
</style>