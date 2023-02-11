<script setup>
  import { ref, computed } from 'vue'

  const player = ref('X')

  const board = ref([
    ['', '', ''],
    ['', '', ''],
    ['', '', '']
  ])

  const filledSquares = ref(0)


  // Calculate winner of the game
  const calculateWinner = (squares) => {
    const winningLines = [
      [0, 1, 2],
      [3, 4, 5],
      [6, 7, 8],
      [0, 3, 6],
      [1, 4, 7],
      [2, 5, 8],
      [0, 4, 8],
      [2, 4, 6]
    ];

    for ( let i = 0; i < winningLines.length; i++ ) {
      const [a, b, c] = winningLines[i];
      if ( squares[a] && squares[a] === squares[b] && squares[a] === squares[c] ) {
        filledSquares.value = 0
        return squares[a];
      }
    }
    return null;
  }


  // Get winner
  const winner = computed( () => calculateWinner(board.value.flat()) )


  // Play turn
  const makeMove = (x,y) => {
    // If we already have a winner
    if ( winner.value ) return

    // If square is not empty
    if ( board.value[x][y] !== '' ) return

    // Make move
    board.value[x][y] = player.value
    filledSquares.value++

    // Swap player value
    player.value = (player.value === 'X') ? 'O' : 'X'
  }


  // Reset the game
  const resetGame = () => {
    board.value = [
      ['', '', ''],
      ['', '', ''],
      ['', '', '']
    ]
    
    player.value = 'X'

    filledSquares.value = 0
  }

</script>


<template>

  <main class="text-center pt-8 bg-gray-800 text-white min-h-screen">
    <h1 class="text-4xl mb-8 font-bold uppercase"> Tic Tac Toe </h1>

    <h3 class="text-xl mb-6">It's {{ player }}'s turn</h3>

    <div class="flex flex-col items-center mb-8">
      <div v-for="(row, x) in board" :key="x" class="flex">
        <div v-for="(cell, y) in row" :key="y" @click="makeMove(x, y)"
             :class="`${ cell == 'X' ? 'text-pink-500' : 'text-yellow-300' } border border-white w-20 h-20 hover:bg-gray-600 flex items-center justify-center material-icons-outlined text-4xl cursor-pointer`">
          {{ cell === 'X' ? 'close' : cell === 'O' ? 'circle' : '' }}
        </div>
      </div>
    </div>

    <h2 v-if="winner" class="text-4xl font-bold mb-8">Player {{ winner }} wins!</h2>
    <h2 v-else-if="!winner && filledSquares == 9" class="text-4xl font-bold mb-8">Nobody wins :(</h2>

    <button @click="resetGame" class="px-4 py-2 bg-pink-500 rounded uppercase font-bold hover:bg-pink-600 duration-300">Reset game</button>
  </main>

</template>


<style scoped>
</style>
