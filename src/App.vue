<script setup>
import { ref, computed, onMounted } from 'vue'
const wins = ref(0)
const draws = ref(0)
const losses = ref(0)
const choice = ref(null)
const computerChoice = ref(null)
const verdict = ref(null)
const outcomes = {
	'rock': {
		'rock': 'draw',
		'paper': 'loss',
		'scissors': 'win'
	},
	'paper': {
		'rock': 'win',
		'paper': 'draw',
		'scissors': 'loss'
	},
	'scissors': {
		'rock': 'loss',
		'paper': 'win',
		'scissors': 'draw'
	}
}
const play = (c) => {
	choice.value = c
	
	const choices = ['rock', 'paper', 'scissors']
	const random = Math.floor(Math.random() * choices.length)
	computerChoice.value = choices[random]
	const outcome = outcomes[choice.value][computerChoice.value]
	
	if (outcome === 'win') {
		wins.value++
		verdict.value = 'You win!'
	} else if (outcome === 'loss') {
		losses.value++
		verdict.value = 'You Lose!'
	} else {
		draws.value++
		verdict.value = 'It\'s a draw'
	}
	SaveGame()
}
const winPercentage = computed(() => {
	const total = wins.value + draws.value + losses.value
	return total ? (wins.value / total) * 100 : 0
})
const SaveGame = () => {
	localStorage.setItem('wins', wins.value)
	localStorage.setItem('draws', draws.value)
	localStorage.setItem('losses', losses.value)
}
const LoadGame = () => {
	wins.value = parseInt(localStorage.getItem('wins')) || 0
	draws.value = parseInt(localStorage.getItem('draws')) || 0
	losses.value = parseInt(localStorage.getItem('losses')) || 0
}
const ResetRound = () => {
	choice.value = null
	computerChoice.value = null
	verdict.value = null
}
onMounted(() => {
	LoadGame()
	window.addEventListener('keypress', (e) => {
		if (e.key === 'r') {
			ResetRound()
		}
	})
})
</script>

<template>
  <div class="bg-gray-400 dark:bg-gray-800 text-gray-600 dark:text-white text-center min-h-screen felx felx-col">
    <header class=" container mx-auto p-6">
      <h1 class="text-4xl font-bold manga">Rock, Paper, Scissors!</h1>
    </header>
    
    <main class="container p-6 flex-1 mx-auto">
      <div v-if="choice === null" class="felx items-center justify-center -mx-6 flex-row">

        <button @click="play('rock')"
					class="bg-white rounded-full shadow-lg w-32 p-6 sm:w-64 sm:p-12  transition-colors duration-300 hover:bg-pink-500">
					<img src="./assets/RockIcon.svg" alt="Rock" class="w-full" />
				</button>

				<button @click="play('paper')"
					class="bg-white rounded-full shadow-lg w-32 p-6 sm:w-64 sm:p-12 m-6 transition-colors duration-300 hover:bg-green-500">
					<img src="./assets/PaperIcon.svg" alt="Paper" />
				</button>

				<button @click="play('scissors')"
					class="bg-white rounded-full shadow-lg w-32 p-6 sm:w-64 sm:p-12  transition-colors duration-300 hover:bg-yellow-500">
					<img src="./assets/ScissorsIcon.svg" alt="Scissors" />
				</button>

      </div>

      <div v-else>
				<div class="text-3xl mb-4">
					You picked <span class="text-pink-500">{{ choice }}</span>
				</div>
				<div class="text-3xl mb-4">
					The computer picked <span class="text-green-700 dark:text-green-500">{{ computerChoice }}</span>
				</div>
				<div class="text-6xl mb-12 manga">
					{{ verdict }}
				</div>

				<button @click="ResetRound" class="bg-blue-500 text-lg py-2 px-4 text-white shadow-md rounded-lg">Reset</button>
			</div>

			<div class="mt-12 text-3xl mb-4">{{ wins }} : {{ draws }} : {{ losses }}</div>

			<div class="text-lg">Win rate: {{ Math.round(winPercentage) }}%</div>
    </main>

    <footer class="container mx-auto p-6">
			<a href="https://www.vecteezy.com/free-vector/rock-paper-scissors-game" target="_blank">Vectors by	Vecteezy</a>
		</footer>
  </div>
</template>

<style>
.manga{
	color: transparent;
	background: rgb(255,130,62);
	background: linear-gradient(90deg, rgba(255,130,62,1) 0%, rgba(238,33,109,1) 50%, rgba(219,0,255,1) 100%);
	background-clip: text;
	animation: move 7s linear infinite;
	background-size: 500%;

}

@keyframes move {
	0%{
		background-position: 0 100%;
	}
	
	50%{
		background-position: 100% 0;
	}

	100%{
		background-position: 0 100%;
	}
}
</style>
