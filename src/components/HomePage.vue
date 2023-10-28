<template>
  <div class="page">
  
  
    <div class="navbar">Hangman.ai</div>
  
  
    <div class="playground">
  
  
      <div class="prompt">
        <img src="../assets/walder.png" alt="" height="250" width="250">
        <input class="input" placeholder="Guess the prompt!" @keyup.enter="handleGuess" v-model="userGuessPrime"/>
      </div>
  
  
      <div class="guesses" v-for="guess in guessList" :key="guess">
        <!--This will display da guesses-->
        <div class="guess">Your guess: {{ guess }} | Accuracy: {{ guessAccuracy }}% </div>
      </div>
  
  
      <div class="answer" v-if="gameDone"> The right prompt was {{ prompt }}</div>
  
  
      <div class="tests"> User guess: {{ userGuess }}, Guess list: {{ guessList }}</div>
    </div>
  
  
    <div class="footer">Find me here -> Tiktok | Twitter </div>
  
  
  </div>
  </template>
  
  
  <script setup>
  import { computed } from "vue";
  import { ref } from "vue"
  
  
    const prompt = 'Walter White smoking it up dawggg'.toLowerCase()
  
  
    let userGuessPrime = ref('')
    let userGuess = computed(()=>userGuessPrime.value.toLowerCase())
    let guessList = []
    let guessAccuracy = null
  
  
    // let lastGuess = 'users last guess from array'
  
  
    const handleGuess = () => {
      guessList.push(userGuess.value),
      checkDone(), userGuessPrime.value = ''
      console.log('Current guess list:', guessList)
      }
  
  
  
  
    let gameDone = false
    let checkDone = () => {
      checkGuessAccuracy()
      console.log('Game state has been checked.');
      if (guessList.length == 6)
      {
        gameDone = true, console.log('Out of tries!')
        }
      else if (guessAccuracy >= 80){
        gameDone = true, console.log('You won!')
      }
      }  
  
  
      // le function
    let checkGuessAccuracy = () => {
        // Guess accuracy logic
  
  
          // create promptWords array with each word as an element
      const promptWords = prompt.split(" ")
          // get the previous guess in guessList array
      let lastGuess = guessList[guessList.length - 1]
          // make the previous guess into an array to be checked against prompt words
      const lastGuessChecker = lastGuess.split(" ")
          // store the correct words
      const correctWords = []
          // calculate guess accuracy percentage
  
  
     
      promptWords.forEach(word => {
        if (lastGuessChecker.includes(word)) {
          correctWords.push(word)
        }
      })
      console.log('Prompt word array:', {promptWords}, 'Correct words:', {correctWords}),
      guessAccuracy = (correctWords.length / promptWords.length) * 100
    }
  




</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
