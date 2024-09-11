<!-- XOGame.vue -->
<template>
    <div class="xo-game">
      <h2>Tic-Tac-Toe</h2>
      <div class="board">
        <div v-for="(row, rowIndex) in board" :key="rowIndex" class="row">
          <button v-for="(cell, colIndex) in row"
            :key="colIndex"
            @click="makeMove(rowIndex, colIndex)"
            :disabled="cell !== '' || gameOver"
            class="cell">
            {{ cell }}
          </button>
        </div>
      </div>
      <p>Current Player: {{ currentPlayer }}</p>
      <p v-if="gameOver">
        {{ winner ? `Player ${winner} wins!` : 'It\'s a draw!' }}
      </p>
      <button @click="resetGame" class="reset-btn">Reset Game</button>
    </div>
  </template>
  
  <script>
  export default {
    name: 'XOGame',
    data() {
      return {
        board: [
          ['', '', ''],
          ['', '', ''],
          ['', '', '']
        ],
        currentPlayer: 'X',
        gameOver: false,
        winner: null
      }
    },
    methods: {
      makeMove(row, col) {
        if (this.board[row][col] === '' && !this.gameOver) {
          this.$set(this.board[row], col, this.currentPlayer)
          if (this.checkWin(row, col)) {
            this.gameOver = true
            this.winner = this.currentPlayer
          } else if (this.checkDraw()) {
            this.gameOver = true
          } else {
            this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X'
          }
        }
      },
      checkWin(row, col) {
        const player = this.board[row][col]
        if (this.board[row].every(cell => cell === player)) return true
        if (this.board.every(r => r[col] === player)) return true
        if (row === col && this.board.every((r, i) => r[i] === player)) return true
        if (row + col === 2 && this.board.every((r, i) => r[2 - i] === player)) return true
        return false
      },
      checkDraw() {
        return this.board.every(row => row.every(cell => cell !== ''))
      },
      resetGame() {
        this.board = [
          ['', '', ''],
          ['', '', ''],
          ['', '', '']
        ]
        this.currentPlayer = 'X'
        this.gameOver = false
        this.winner = null
      }
    }
  }
  </script>
  
  <style scoped>
  .xo-game {
    font-family: Arial, sans-serif;
    text-align: center;
  }
  
  .board {
    display: inline-block;
    margin-top: 20px;
  }
  
  .row {
    display: flex;
  }
  
  .cell {
    width: 60px;
    height: 60px;
    font-size: 24px;
    font-weight: bold;
    margin: 2px;
    cursor: pointer;
  }
  
  .reset-btn {
    margin-top: 20px;
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
  }
  </style>