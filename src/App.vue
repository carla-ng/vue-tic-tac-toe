<script setup>
  import { ref, computed } from 'vue'

  const player = ref('X')
  const board = ref([
    ['', '', ''],
    ['', '', ''],
    ['', '', '']
  ])


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
        return squares[a];
      }
    }
    return null;
  }


  // Get winner
  const winner = computed( () => calculateWinner(board.value.flat()) )


  // Play turn
  const makeMove = (x,y) => {
    if ( winner.value ) return
    if ( board.value[x][y] !== '' ) return
    board.value[x][y] = player.value
  }

</script>

<template>

  <h1>Hello World</h1>

</template>

<style scoped>
</style>
