<script>
import { ref } from "vue";
export default {
  data() {
    return {
      board: [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""],
      ],
      currentPlayer: "X",
      winner: null,
      isTie: false,
      gameOver: false,
      moveCount: 0,
    };
  },
  methods: {
    makeMove(row, col) {
      if (!this.board[row][col] && !this.winner) {
        this.board[row][col] = this.currentPlayer;
        this.moveCount++;
        if (this.checkWin()) {
          this.winner = this.currentPlayer;
        } else if (this.checkTie()) {
          this.isTie = true;
        } else {
          this.currentPlayer = this.currentPlayer === "X" ? "O" : "X";
        }
      }
    },
    checkWin() {
      for (let i = 0; i < 3; i++) {
        if (
          this.board[i][0] === this.currentPlayer &&
          this.board[i][1] === this.currentPlayer &&
          this.board[i][2] === this.currentPlayer
        ) {
          return true;
        }
        if (
          this.board[0][i] === this.currentPlayer &&
          this.board[1][i] === this.currentPlayer &&
          this.board[2][i] === this.currentPlayer
        ) {
          return true;
        }
      }
      if (
        this.board[0][0] === this.currentPlayer &&
        this.board[1][1] === this.currentPlayer &&
        this.board[2][2] === this.currentPlayer
      ) {
        return true;
      }
      if (
        this.board[0][2] === this.currentPlayer &&
        this.board[1][1] === this.currentPlayer &&
        this.board[2][0] === this.currentPlayer
      ) {
        return true;
      }
      return false;
    },
    checkTie() {
      for (let i = 0; i < 3; i++) {
        for (let j = 0; j < 3; j++) {
          if (!this.board[i][j]) {
            return false;
          }
        }
      }
      return true;
    },
    reset() {
      this.board = [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""],
      ];
      this.currentPlayer = "X";
      this.gameOver = false;
      this.winner = null;
      this.moveCount = 0;
      this.isTie = false;
    },
  },
};
</script>

<template>
  <div>
    <h1 class="text-5xl bg-orange-500 rounded-3xl mb-8">Tic Tac Toe</h1>
    <p class="bg-blue-300 rounded-xl text-black font-bold mb-8">
      Current player: {{ currentPlayer }} <br />
      Moves: {{ moveCount }}
    </p>
    <div class="flex flex-col items-center mb-8 border-black">
      <div class="flex" v-for="(row, rowIndex) in board" :key="rowIndex">
        <div
          class="flex"
          v-for="(cell, cellIndex) in row"
          :key="cellIndex"
          @click="makeMove(rowIndex, cellIndex)"
          :class="`border border-black w-24 h-24 bg-yellow-300 hover:bg-yellow-600 hover:text-5xl flex items-center justify-center material-icons-outlined text-4xl cursor-pointer 
          ${cell === 'X' ? 'text-red-500' : 'text-blue-400'}`"
          :disabled="cell !== null"
        >
          {{ cell }}
        </div>
      </div>
    </div>
    <p
      class="py-4 px-2 font-bold text-6xl text-red-500 mb-8"
      v-if="winner === 'X'"
    >
      {{ winner }} wins!
    </p>
    <p
      class="py-4 px-2 font-bold text-6xl text-blue-400 mb-8"
      v-else-if="winner === 'O'"
    >
      {{ winner }} wins!
    </p>

    <p class="py-4 px-2 font-bold text-6xl text-blue-600 mb-8" v-if="isTie">
      It's a tie!
    </p>
    <br />
    <button
      class="text-black bg-blue-300 hover:bg-blue-400 mb-8 rounded-2xl font-bold py-4 px-2 duration-300"
      @click="reset"
    >
      Reset game!
    </button>
  </div>
</template>

<style>
body {
  background-color: rgba(255, 196, 0, 0.406);
  margin: 0;
  display: flex;
  min-width: 320px;
  min-height: 100vh;
}

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;
  text-align: center;
}

.tic-tac-toe {
  text-align: center;
}

.board {
  width: 309px;
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
}

.cell {
  width: 100px;
  height: 100px;
  border-radius: 10px;
  margin: 3px;
  align-items: center;
  display: flex;
  justify-content: center;
  cursor: pointer;
  font-size: 40px;
  background-color: #48cae4;
}

.cell:hover {
  opacity: 70%;
}

.cell-x {
  color: #03045e;
}

.cell-o {
  color: #fca311;
}

.label {
  font-size: 20px;
  font-weight: 600;
}
</style>
