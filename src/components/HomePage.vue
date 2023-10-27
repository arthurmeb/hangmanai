<template>
<div class="page">

  <div class="navbar">Hangman.ai</div>

  <div class="playground">

    <div class="prompt">
      <img src="@/assets/walder.png" alt="" height="250" width="250">
      <input class="input" placeholder="Guess the prompt!" @keyup.enter="handleGuess" v-model="userGuess"/>
    </div>

    <div class="guesses" v-for="guess in guessList" :key="guess">
      <!--This will display da guesses-->
      <div class="guess">Your guess: {{ guess }} </div>
    </div>

    <div class="answer" v-if="gameDone"> The right prompt was {{ prompt }}</div>

    <div class="tests"> User guess: {{ userGuess }}, Guess list: {{ guessList }}</div>
  </div>

  <div class="footer">Find me here -> Tiktok | Twitter </div>

</div>
</template>

<script setup>
import { ref } from "vue"

  const prompt = 'Walter White smoking it up dawggg'

  let userGuess = ref('')
  let guessList = []
  // let lastGuess = 'users last guess from array'

  const handleGuess = () => {guessList.push(userGuess.value), checkDone(), userGuess.value = '', console.log('enter pressed', guessList)}

  // Guess accuracy

  // let checkAccuracy = crazy javascripting 
  let guessAccuracy = 20

  let gameDone = false
  let checkDone = () => {console.log('Game checked'); if (guessList.length == 6 || guessAccuracy >= 80) {gameDone = true, console.log('Game is done!')}}
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
