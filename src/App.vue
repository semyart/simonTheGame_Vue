<template>
  <div class="app">
    <div class="square-container">
      <gameSquare
        @getColor="addToArray"
        color="blue"
        :timeBetweenActivation="this.timeBetweenActivation"
      />
      <gameSquare
        @getColor="addToArray"
        color="red"
        :timeBetweenActivation="this.timeBetweenActivation"
      />
      <gameSquare
        @getColor="addToArray"
        color="yellow"
        :timeBetweenActivation="this.timeBetweenActivation"
      />
      <gameSquare
        @getColor="addToArray"
        color="green"
        :timeBetweenActivation="this.timeBetweenActivation"
      />
    </div>

    <div class="btn-container">
      <div class="lvl-container">
        <span class="lvl-span">Выберите уровень игры:</span>
        <label class="lvl-label">
          Легкий
          <input
            selected
            type="radio"
            name="game-level"
            value="Легкий"
            v-model="gameDifficulty"
            @change="selectGameDifficult"
          />
        </label>
        <label class="lvl-label">
          Средний
          <input
            type="radio"
            name="game-level"
            value="Средний"
            v-model="gameDifficulty"
            @change="selectGameDifficult"
          />
        </label>
        <label class="lvl-label">
          Сложный
          <input
            type="radio"
            name="game-level"
            value="Сложный"
            v-model="gameDifficulty"
            @change="selectGameDifficult"
          />
        </label>
      </div>
      <button @click="startGame">Начать игру</button>
    </div>
  </div>
</template>

<script>
import gameSquare from "./components/game-square.vue";

export default {
  name: "App",
  components: {
    gameSquare,
  },
  data() {
    return {
      gameArray: [],
      playerArray: [],
      colors: ["blue", "red", "yellow", "green"],
      numberOfClicks: 0,
      gameLevel: 0,
      gameDifficulty: "Легкий",
      timeBetweenActivation: 1300,
    };
  },
  methods: {
    addRandomColor() {
      const randomNumber = Math.floor(Math.random() * this.colors.length);
      this.gameArray.push(this.colors[randomNumber]);
    },

    startGame() {
      this.gameArray.length = 0;
      this.playingGame();
    },

    async playingGame() {
      this.numberOfClicks = 0;
      this.playerArray.length = 0;
      this.gameLevel += 1;
      this.addRandomColor();
      for (const color of this.gameArray) {
        for (const el of this.$children) {
          if (el.color === color) {
            el.active = true;
            await new Promise((resolve) =>
              setTimeout(resolve, this.timeBetweenActivation)
            );
            el.active = false;
            await new Promise((resolve) => setTimeout(resolve, 200));
          }
        }
      }
    },

    addToArray(color) {
      this.numberOfClicks += 1;
      this.playerArray.push(color);
    },

    selectGameDifficult() {
      if (this.gameDifficulty === "Легкий") {
        this.timeBetweenActivation = 1500;
      } else if (this.gameDifficulty === "Средний") {
        this.timeBetweenActivation = 900;
      } else {
        this.timeBetweenActivation = 300;
      }
    },
  },
  watch: {
    async playerArray() {
      for (let i = 0; i < this.numberOfClicks; i++) {
        if (this.gameArray[i] !== this.playerArray[i]) {
          this.gameArray.length = 0;
          this.playerArray.length = 0;
          this.gameLevel = 0;
          this.numberOfClicks = 0;
          alert("Game over!");
        } else {
          if (
            this.numberOfClicks === this.gameLevel &&
            i === this.numberOfClicks - 1
          ) {
            await new Promise((resolve) =>
              setTimeout(resolve, this.timeBetweenActivation + 100)
            );
            this.playingGame();
          }
        }
      }
    },
  },
};
</script>

<style>
@import "@/assets/normalize.css";
@import "@/assets/style.css";

.app {
  display: flex;
  margin: 0 auto;
  margin-top: 10vh;
  width: fit-content;
  align-items: center;
}

.square-container {
  display: flex;
  width: 400px;
  flex-wrap: wrap;
}

.btn-container {
  margin-left: 2vw;
}

.lvl-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}

.lvl-span {
  margin-bottom: 7px;
}

.lvl-label {
  margin-bottom: 3px;
}
</style>
