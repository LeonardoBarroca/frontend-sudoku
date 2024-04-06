<template>
  <div class="sudoku-game">
    <h1>Sudoku 4x4</h1>
    <div class="sudoku-board">
      <div class="sudoku-table">
        <div v-for="(row, rowIndex) in sudoku" :key="rowIndex" class="row">
          <div v-for="(cell, colIndex) in row" :key="colIndex" :class="['cell', { 'fixed-cell': cell.readonly }]">
            <input type="text" :value="cell.value" :readonly="cell.readonly"
              @input="updateCell(rowIndex, colIndex, $event.target.value)">
          </div>
        </div>
      </div>
    </div>
    <div class="new-game">
      <button class="btn btn-outline-secondary" @click="generateNewGame">Novo Jogo</button>
    </div>
    <div class="validade-sudoku">
      <button class="btn btn-primary" @click="submitSudoku" :disabled="!isSudokuComplete">Enviar</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      sudoku: [],
    };
  },
  computed: {
    isSudokuComplete() {
      for (let i = 0; i < this.sudoku.length; i++) {
        for (let j = 0; j < this.sudoku[i].length; j++) {
          if (!this.sudoku[i][j].readonly && !this.sudoku[i][j].value) {
            return false;
          }
        }
      }
      return true;
    }
  },
  created() {
    this.generateNewGame();
  },
  methods: {
    generateNewGame() {
      this.sudoku = this.generateSudoku();
    },
    generateSudoku() {
      const sudoku = [];
      const fixedIndexes = [1, 6, 11, 16];

      const uniqueNumbers = this.generateUniqueNumbers(4);

      for (let i = 0; i < 4; i++) {
        const row = [];
        for (let j = 0; j < 4; j++) {
          let value;
          if (fixedIndexes.includes(i * 4 + j + 1)) {
            value = uniqueNumbers.pop();
          } else {
            value = '';
          }
          row.push({ value: value, readonly: fixedIndexes.includes(i * 4 + j + 1) });
        }
        sudoku.push(row);
      }
      return sudoku;
    },
    generateUniqueNumbers(count) {
      const numbers = [];
      while (numbers.length < count) {
        const num = this.generateRandomNumber();
        if (!numbers.includes(num)) {
          numbers.push(num);
        }
      }
      return numbers;
    },
    generateRandomNumber() {
      return Math.floor(Math.random() * 4) + 1;
    },
    updateCell(rowIndex, colIndex, value) {
      const newValue = value.replace(/[^1-4]/g, '');
      this.sudoku[rowIndex][colIndex].value = newValue;
    },
    submitSudoku() {
      if (this.isSudokuComplete) {
        const data = this.sudoku.map(row => row.map(cell => parseInt(cell.value)));
        console.log(data)

        fetch('http://localhost:5000/validate-sudoku', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({ sudoku: data })
        })
          .then(response => response.json())
          .then(data => {
            if (data.valid) {
              alert('Sudoku válido!');
            } else {
              alert('Sudoku inválido!');
            }
          })
          .catch(error => {
            console.error('Erro ao enviar Sudoku:', error);
          });
      } else {
        alert('Preencha todas as células do Sudoku antes de enviar.');
      }
    }
  },
};
</script>

<style>
.sudoku-game {
  margin-top: 16px;
  text-align: center;
  flex-direction: column;
}

.sudoku-board {
  margin: 16px;
  display: flex;
  justify-content: center;
}

.row {
  width: 160px;
}

.cell {
  border: 1px solid #000;
  width: 40px;
  height: 40px;
}

.fixed-cell {
  background-color: lightgray;
}

input {
  width: 100%;
  height: 100%;
  font-size: 20px;
  text-align: center;
  border: none;
  outline: none;
  background: transparent;
}

.btn-outline-secondary {
  width: 160px;
}

.btn-primary {
  margin-top: 16px;
  width: 160px;
}
</style>
