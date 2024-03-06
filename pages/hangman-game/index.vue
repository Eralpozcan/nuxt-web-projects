<template>
	<div class="hangman-game">
		<h1>Hangman</h1>
		<p>Find the hidden word - Enter a letter</p>
		<div class="game-container">
			<svg height="250" width="200" class="figure-container">
				<!-- Rod -->
				<line x1="60" y1="20" x2="140" y2="20" />
				<line x1="140" y1="20" x2="140" y2="50" />
				<line x1="60" y1="20" x2="60" y2="230" />
				<line x1="20" y1="230" x2="100" y2="230" />

				<!-- Head -->
				<circle cx="140" cy="70" r="20" class="figure-part" />
				<!-- Body -->
				<line x1="140" y1="90" x2="140" y2="150" class="figure-part" />
				<!-- Arms -->
				<line x1="140" y1="120" x2="120" y2="100" class="figure-part" />
				<line x1="140" y1="120" x2="160" y2="100" class="figure-part" />
				<!-- Legs -->
				<line x1="140" y1="150" x2="120" y2="180" class="figure-part" />
				<line x1="140" y1="150" x2="160" y2="180" class="figure-part" />
			</svg>

			<div class="wrong-letters-container" v-if="wrongLetters.length > 0">
        <p>Wrong</p>
				<div id="wrong-letters"> {{ wrongLetters }}</div>
			</div>

			<div class="word">
        <span v-for="(letter, index) in selectedWord" :key="index" class="letter">
          {{ correctLetters.includes(letter) ? letter : '' }}

          {{ isWon }}
        </span>
      </div>
		</div>

		<!-- Container for final message -->
		<div class="popup-container" :style="{ display: [ isFailed ? 'flex' : 'none'] }">
			<div class="popup">
				<h2 class="final-message">
          Unfortunately you lost. 😕
        </h2>
				<h3 class="final-message-reveal-word">
          {{ `...the word was: ${selectedWord}` }}
        </h3>
				<button @click="playAgain">Play Again</button>
			</div>
		</div>

		<!-- Notification -->
		<div class="notification-container" :class="{'show': isShowNotification}">
			<p>You have already entered this letter</p>
		</div>
	</div>
</template>

<script setup>
useSeoMeta({
	title: 'Hangman Game',
  description: 'This is a simple Hangman-Game'
})
useHead({
	htmlAttrs: {
		lang: 'en'
	}
})
const words = ref(['application','programming','interface','wizard'])
const selectedWord = ref(words.value[Math.floor(Math.random() * words.value.length)])
const playable = ref(true)
const correctLetters = ref([])
const wrongLetters = ref([])
const isShowNotification = ref(false)
const popup = ref(null)
const figureParts = ref(null)


function showNotification() {
	isShowNotification.value = true
	setTimeout(() => {
		isShowNotification.value = false
	},2000);
}

function updateWrongLetters() {

}

function selectWord() {
  selectedWord.value = words[Math.floor(Math.random() * words.length)];
}

function playAgain() {
  playable.value = false
  correctLetters.value = []
  wrongLetters.value = []
  selectWord()
}

const isWon = computed(()=> {
  if (correctLetters.value === selectWord.value) {
    console.log("1")
  }
})

const isFailed = computed(()=> {
  if (wrongLetters.value?.length === figureParts.value?.length) {
    playable.value = false
    return true
  } else return false
})

onMounted(()=> {
  figureParts.value = document.querySelectorAll(".figure-part")
})

useEventListener(document, 'keydown', (e) => {
  console.log(e.keyCode)
	if(playable.value) {
		if(e.keyCode >= 65 && e.keyCode <=90 || e.keyCode === 222){
			const letter = e.key.toLowerCase()
      console.log(letter)
      console.log(selectedWord.value.includes(letter))
			if (selectedWord.value.includes(letter)){
				if (!correctLetters.value.includes(letter)) {
					correctLetters.value.push(letter)
				} else showNotification()
			} else {
        if (!wrongLetters.value.includes(letter)) {
          wrongLetters.value.push(letter)
          updateWrongLetters()
        } else showNotification()
      }
		} 
	}
});


</script>

<style scoped>
.hangman-game {
  background-color: #34495e;
  color: #fff;
  font-family: Arial, Helvetica, sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 80vh;
  margin: 0;
  overflow: hidden;
}

h1 {
  margin: 20px 0 0;
}

.game-container {
  padding: 20px 30px;
  position: relative;
  margin: auto;
  height: 350px;
  width: 450px;
}

.figure-container {
  fill: transparent;
  stroke: #fff;
  stroke-width: 4px;
  stroke-linecap: round;
}

.figure-part {
  display: none;
}

.wrong-letters-container {
  position: absolute;
  top: 20px;
  right: 20px;
  display: flex;
  flex-direction: column;
  text-align: right;
}

.wrong-letters-container p {
  margin: 0 0 5px;
}

.wrong-letters-container span {
  font-size: 24px;
}

.word {
  display: flex;
  position: absolute;
  bottom: 10px;
  left: 50%;
  transform: translateX(-50%);
}

.letter {
  border-bottom: 3px solid #2980b9;
  display: inline-flex;
  font-size: 30px;
  align-items: center;
  justify-content: center;
  margin: 0 3px;
  height: 50px;
  width: 20px;
}

.popup-container {
  background-color: rgba(0, 0, 0, 0.3);
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  align-items: center;
  justify-content: center;
}

.popup {
  background: #2980b9;
  border-radius: 5px;
  box-shadow: 0 15px 10px 3px rgba(0, 0, 0, 0.1);
  padding: 20px;
  text-align: center;
}

.popup button {
  cursor: pointer;
  background-color: #fff;
  color: #2980b9;
  border: 0;
  margin-top: 20px;
  padding: 12px 20px;
  font-size: 16px;
}

.popup button:active {
  transform: scale(0.98);
}

.popup button:focus {
  outline: 0;
}

.notification-container {
  background-color: rgba(0, 0, 0, 0.3);
  border-radius: 10px 10px 0 0;
  padding: 15px 20px;
  position: absolute;
  bottom: -50px;
  transition: transform 0.3s ease-in-out;
}

.notification-container p {
  margin: 0;
}

.notification-container.show {
  transform: translateY(-50px);
}



</style>