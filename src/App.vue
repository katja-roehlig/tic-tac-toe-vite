<script>
export default {
  data() {
    return {
      currentPlayer: "x",
      states: [" ", " ", " ", " ", " ", " ", " ", " ", " "],
      winner: false,
    };
  },
  computed: {
    isClosing() {
      return (this.winner = false);
    },
  },
  methods: {
    playGame(index) {
      if (this.states[index] === " ") {
        this.states[index] = this.currentPlayer;
      }
    },
    changePlayer() {
      if (this.currentPlayer === "x") {
        this.currentPlayer = "o";
      } else {
        this.currentPlayer = "x";
      }
    },

    compareState() {
      if (
        (this.states[0] !== " " &&
          this.states[0] === this.states[1] &&
          this.states[0] === this.states[2]) ||
        (this.states[3] !== " " &&
          this.states[3] === this.states[4] &&
          this.states[3] === this.states[5]) ||
        (this.states[6] !== " " &&
          this.states[6] === this.states[7] &&
          this.states[6] === this.states[8]) ||
        (this.states[0] !== " " &&
          this.states[0] === this.states[3] &&
          this.states[0] === this.states[6]) ||
        (this.states[1] !== " " &&
          this.states[1] === this.states[4] &&
          this.states[1] === this.states[7]) ||
        (this.states[2] !== " " &&
          this.states[2] === this.states[5] &&
          this.states[2] === this.states[8]) ||
        (this.states[0] !== " " &&
          this.states[0] === this.states[4] &&
          this.states[0] === this.states[8]) ||
        (this.states[2] !== " " &&
          this.states[2] === this.states[4] &&
          this.states[2] === this.states[6])
      ) {
        return (this.winner = true);
      } else {
        this.changePlayer();
      }
    },
    resetGame() {
      return (this.states = [" ", " ", " ", " ", " ", " ", " ", " ", " "]);
    },
  },
};
</script>

<template>
  <main class="main__container">
    <h1 class="heading">TIC TAC TOE</h1>
    <div class="play__container" @click="compareState()">
      <div
        v-for="(state, index) in states"
        :key="index"
        @click="playGame(index)"
        class="item"
        :class="'player-' + state"
      >
        {{ state }}
      </div>
    </div>
    <div
      class="winning__container"
      v-if="winner"
      @click="isClosing, resetGame()"
    >
      <h2 class="winning__message">
        <span :class="'player-' + currentPlayer">{{ currentPlayer }}</span>
        <br />
        hat<br />
        gewonnen !<br /><span class="emoji">🥳</span>
      </h2>
    </div>
    <button class="btn-reset" @click="resetGame()">Reset</button>
  </main>
</template>

<style scoped>
.main__container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: black;
  width: 40rem;
  height: 95vh;
  margin: 1rem auto;
  border: 2px solid blueviolet;
}
.heading {
  font-size: 3.5 rem;
  padding-block: 2rem 1.5rem;
  margin: 0 auto;
  color: #df4bc1;
  text-align: center;
}
.play__container {
  width: 30rem;
  height: 30rem;
  margin: 0 auto;
  padding: 1rem;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  justify-content: center;
  align-items: center;
  background-color: black;
}
.item {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 8rem;
  width: 8rem;
  border: 2px solid blueviolet;
  margin: 1rem;
  background-color: rgb(27, 27, 27);
  font-size: 4rem;
  text-transform: uppercase;
}
.item:hover {
  border-color: #df4bc1;
}
.player-x {
  color: #ffa500;
}
.player-o {
  color: rgb(12, 238, 250);
}
.winning__container {
  display: flex;
  align-items: center;
  justify-content: center;
  color: #df4bc1;
  line-height: 7rem;
  text-align: center;
  text-transform: uppercase;
  width: 40rem;
  height: 95vh;
  border: 2px solid #df4bc1;
  background-color: rgba(46, 1, 87, 0.874);
  position: absolute;
  top: 1rem;
}
.winning__message {
  animation: animate-winning-message 0.6s ease-out 0.1s 1 both;
  font-size: 3.5rem;
}
@keyframes animate-winning-message {
  0% {
    transform: translateY(-60vh);
    opacity: 0;
  }
  100% {
    transform: translateY(-5vh);
    opacity: 1;
  }
}
.emoji {
  font-size: 6rem;
  line-height: 10rem;
}
.btn-reset {
  color: aquamarine;
  background-color: black;
  font-size: 1.6rem;
  text-transform: uppercase;
  border: 2px solid #df4bc1;
  padding: 1rem 2rem;
  margin: 3rem auto;
}
</style>
