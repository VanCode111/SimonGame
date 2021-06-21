<template>
  <div class="game">
    <h1 class="game__title">Simon The Game</h1>
    <div class="game__main">
      <div class="game__container">
        <div class="game__main-wrapper">
          <Board
            @selectField="selectField($event)"
            :fields="fields"
            :gameStatus="gameStatus"
          />
          <div class="game__controls">
            <p class="game__round">Раунд: {{ raund }}</p>
            <button class="game__start" :onClick="start">
              Start
            </button>
            <div class="game__levels">
              <div
                v-for="(item, index) in levels"
                :key="index"
                class="game__level"
              >
                <input
                  type="radio"
                  :value="item.value"
                  :disabled="gameStatus != 'started'"
                  v-model="level"
                />
                <label>{{ item.type }}</label>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Board from "../components/Board.vue";
import { onBeforeMount, ref } from "vue";

export default {
  name: "Game",

  components: {
    Board,
  },
  setup() {
    let difficult = ref(1);
    let level = ref(1500);
    const levels = [
      {
        value: 1500,
        type: "Легкий",
      },
      {
        value: 1000,
        type: "Средний",
      },
      {
        value: 400,
        type: "Сложный",
      },
    ];
    let winFields = ref([]);
    let raund = ref(1);
    let gameStatus = ref("started");
    const colorFields = ["dodgerblue", "#FF5643", "#FEEF33", "#BEDE15"];
    const sounds = [
      require("../assets/sounds/sounds1.mp3"),
      require("../assets/sounds/sounds_2.mp3"),
      require("../assets/sounds/sounds_3.mp3"),
      require("../assets/sounds/sounds_4.mp3"),
    ];
    let fields = ref([]);
    const init = () => {
      fields.value = [];
      raund.value = 1;
      difficult.value = 1;
      gameStatus.value = "started";
      winFields.value = [];
      for (let i = 0; i < colorFields.length; i++) {
        fields.value.push({
          id: i,
          color: colorFields[i],
          value: false,
          sound: new Audio(sounds[i]),
        });
      }
    };
    onBeforeMount(init);
    return {
      difficult,
      fields,
      init,
      raund,
      colorFields,
      level,
      gameStatus,
      winFields,
      levels,
    };
  },
  methods: {
    selectField(id) {
      this.fields[id].value = 1;
      this.fields[id].sound.currentTime = 0;
      this.fields[id].sound.play();
      setTimeout(() => {
        this.fields[id].value = 0;
        this.checkGame(id);
      }, 200);
    },
    checkGame(id) {
      if (this.winFields[0] != id) {
        this.lose();
      } else {
        console.log(this.winFields.length);
        this.winFields.shift();
        if (this.winFields.length == 0) {
          this.win();
        }
      }
    },
    lose() {
      this.init();
      alert("К сожалению, вы проиграли");
    },
    win() {
      this.difficult += 1;
      this.raund += 1;
      this.gameStatus = "started";
      setTimeout(() => {
        this.start();
      }, 2000);
    },
    start() {
      if (this.gameStatus == "process") {
        this.init();
      }
      this.winFields = [];
      this.gameProcess();
    },
    gameProcess() {
      this.gameStatus = "preview";
      this.gameLoop();
    },
    gameLoop(i = 0) {
      const index = Math.floor(Math.random() * 4);
      this.winFields.push(index);
      this.fields[index].value = 1;
      this.fields[index].sound.currentTime = 0;
      this.fields[index].sound.play();
      setTimeout(() => {
        i++;
        this.fields[index].value = 0;
        setTimeout(() => {
          if (i < this.difficult) {
            this.gameLoop(i);
          } else {
            this.gameStatus = "process";
          }
        }, this.level / 2);
      }, this.level / 2);
    },
  },
};
</script>

<style lang="scss" scoped>
.game {
  &__main-wrapper {
    display: flex;
    justify-content: space-between;
  }
  &__title {
    text-align: center;
    font-size: 1.5em;
    font-weight: bold;
  }
  &__main {
    padding-top: 60px;
  }
  &__container {
    max-width: 500px;
    margin: 0 auto;
  }
  &__start {
    width: 5em;
    box-sizing: border-box;
    font-size: 1.4em;
    cursor: pointer;
    margin: 20px 0px;
    -webkit-border-radius: 10px 10px 10px 10px;
    border-radius: 10px 10px 10px 10px;
    background: #6dabe8;
    border: none;
    padding: 0.3em 0.6em;
  }
}
</style>
