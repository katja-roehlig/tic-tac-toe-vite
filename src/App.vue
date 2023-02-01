<script>
export default {
  data() {
    return {
      currentPlayer: "x",
      states: [" ", " ", " ", " ", " ", " ", " ", " ", " "],
      winner: false,
      onePlayer: false,
    };
  },
  methods: {
    isClosing() {
      console.log("closing?");
      return (this.winner = false);
    },
    setPlayer() {
      this.onePlayer = !this.onePlayer;
    },

    playGame(index) {
      if (this.states[index] === " ") {
        this.states[index] = this.currentPlayer;
      }
    },

    changePlayer() {
      console.log("changePlayer");
      if (this.currentPlayer === "x") {
        this.currentPlayer = "o";
        if (this.onePlayer === true && this.states.includes(" ")) {
          this.findPlayItem();
        }
      } else {
        this.currentPlayer = "x";
      }
    },

    findPlayItem() {
      let randomIndex = Math.round(Math.random() * 9);
      if (this.states[randomIndex] === " ") {
        this.states[randomIndex] = this.currentPlayer;
        console.log(this.states);
        this.compareState();
      } else {
        this.findPlayItem();
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
      this.states = [" ", " ", " ", " ", " ", " ", " ", " ", " "];
      this.currentPlayer = "x";
    },
  },
};
</script>

<template>
  <main class="main__container">
    <h1 class="heading">TIC TAC TOE</h1>
    <div class="btn__container">
      <button @click.prevent="setPlayer" :class="{ active: onePlayer }">
        1 Player
      </button>
      <button @click.prevent="setPlayer" :class="{ active: !onePlayer }">
        2 Player
      </button>
    </div>
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
      @click="isClosing(), resetGame()"
    >
      <h2 class="winning__message">
        <span class="winner" :class="'player-' + currentPlayer">{{
          currentPlayer
        }}</span>
        <br />
        has won!<br /><span class="emoji">🥳</span>
      </h2>
    </div>
    <button class="btn-reset" @click.prevent="resetGame()">Reset</button>
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
  height: 100%;
  margin: 0rem auto;
  border: 3px solid blueviolet;
}
.btn__container {
  width: 28rem;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
.active {
  border: 2px solid aquamarine;
  color: aquamarine;
}
.heading {
  font-size: 3.5 rem;
  padding: 4rem 1.5rem 3rem 1.5rem;
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
.winner {
  text-transform: uppercase;
}
.player-x {
  color: #ffa500;
}
.player-o {
  color: aquamarine;
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
  height: 100%;
  border: 2px solid #df4bc1;
  background-color: rgba(46, 1, 87, 0.874);
  position: absolute;
  top: 0rem;
  font-size: 2rem;
}
.winning__message {
  animation: animate-winning-message 0.6s ease-out 0.1s 1 both;
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
button {
  color: #df4bc1;
  background-color: black;
  font-size: 1.3rem;
  text-transform: uppercase;
  border: 2px solid blueviolet;
  padding: 0.8rem 2rem;
}
.btn-reset {
  margin: 3rem auto 8rem auto;
}
button:hover {
  border-color: aquamarine;
  color: aquamarine;
}

@media (max-width: 768px) {
  .main__container {
    width: 32rem;
    margin: 0 auto;
  }
  .btn__container {
    width: 19rem;
  }
  button {
    font-size: 1rem;
    padding: 0.5rem 1rem;
  }
  .play__container {
    width: 22rem;
    height: 22rem;
  }
  .item {
    width: 5rem;
    height: 5rem;
  }
  .winning__container {
    width: 36rem;
    height: 100%;
    font-size: 1.5rem;
    line-height: 4rem;
  }
}
</style>
