<script setup>
import { ref, computed } from 'vue'

const player = ref('X')
const board = ref([
  ['', '', ''],
  ['', '', ''],
  ['', '', '']
])
const CalculateWinner = (board) => {
  const lines = [[0, 1, 2],[3, 4, 5],[6, 7, 8],[0, 3, 6],[1, 4, 7],[2, 5, 8],[0, 4, 8],[2, 4, 6]]

  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i]

    if (board[a] && board[a] === board[b] && board[a] === board[c]) {
      return board[a]
    }
  }

  return null
}

const winner = computed(() => CalculateWinner(board.value.flat()));

const MakeMove = (x, y) => {
	if (winner.value) return

	if (board.value[x][y]) return

	board.value[x][y] = player.value

	player.value = player.value === 'X' ? 'O' : 'X'
}

const ResetGame = () => {
	board.value = [
		['', '', ''],
		['', '', ''],
		['', '', '']
	]
	player.value = 'X'
}

</script>

<template>
  <main>
		<h1>TIC TAC TOE</h1>

		<h3>Player {{ player }}'s turn</h3>

		<div class="container">
			<div class="con1"
				v-for="(row, x) in board" 
				:key="x">
				<div class="con2" 
					v-for="(cell, y) in row" 
					:key="y" 
					@click="MakeMove(x, y)" 
					:class="`material-icons-outlined ${cell === 'X' ? 'text-pink-500' : 'text-blue-400'}`">
					{{ cell === 'X' ? 'close' : cell === 'O' ? 'circle' : '' }}
				</div>
			</div>
		</div>

		<div>
			<h2 v-if="winner">Player '{{ winner }}' wins!</h2>
      <button @click="ResetGame">Reset Game</button>
		</div>
	</main>
</template>

<style>
</style>
