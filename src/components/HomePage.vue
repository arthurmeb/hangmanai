<template>
  <div class="page">
  
  
    <div class="navbar">Hangman.ai</div>
  
  
    <div class="playground">
  
      <div class="prompt">

        <div> Your right words:
          <div class="hint" v-for="word in allCorrectWords" :key="word">{{ word }}</div>
        </div>
        <div>
          <img src="../assets/walder.png" alt="" height="250" width="250">
        <input class="input" placeholder="Guess the prompt!" @keyup.enter="handleGuess" v-model="userGuessPrime" :disabled="gameDone"/>
      </div>

      </div>
  
      <div class="guesses" v-for="(answer) in guessList" :key="answer">
        <!--This will display da guesses-->
        <div class="inline">Your guess:
            <p class="inline" v-for="word in answer.guessWords" :key="word" :style="answerStyles(word)">
            {{ word }}
           </p>
        | Accuracy: <span :style="styleAccuracy(answer)">{{ answer.accuracy }}%</span> 
        </div>
      </div>
  
  
      <div class="answer" v-if="gameDone"> The right prompt was {{ prompt }}</div>
  
    </div>
  
  
    <div class="footer">Find me here -> Tiktok | Twitter </div>
  
  
  </div>
  </template>
  
  
  <script setup>
  import { computed} from "vue";
  import { ref } from "vue"
  
  
    const prompt = 'Walter White smoking it up dawggg'.toLowerCase()
  
  
    let userGuessPrime = ref('')
    let userGuess = computed(()=>userGuessPrime.value.toLowerCase())
    let guessList = []
    let guessAccuracy = ref(null)
    let lastGuess
    let correctWords
    let allCorrectWords = ref([])
    let answerWords
    // Log user's guess => check if game is finished => check guess accuracy
  
    const handleGuess = () => {
      answerWords = userGuess.value.split(" ")      
      guessList.push({
        guess: userGuess.value,
        accuracy: guessAccuracy.value,
        guessWords: answerWords,
        rightWords: correctWords
      }),
      checkGuessAccuracy()

      console.log('Answer words:', answerWords)
      

      userGuessPrime.value = ''

      console.log('Current guess list:', guessList, 'accuracy:', lastGuess.accuracy)




      }

      let checkGuessAccuracy = () => {

          // create promptWords array with each prompt word as an element
          
      const promptWords = prompt.split(" ")
      
          // get the previous guess in guessList array

      lastGuess = guessList[guessList.length - 1]

          // make the previous guess into an array to be checked against prompt words

      const lastGuessChecker = lastGuess.guess.split(" ")
      
          // store the correct words

      correctWords = []

          // calculate guess accuracy percentage

      promptWords.forEach(word => {
        if (lastGuessChecker.includes(word)) {
          correctWords.push(word)
          if (!allCorrectWords.value.includes(word)){
          allCorrectWords.value.push(word)            
          }
        }
      })

      console.log('Prompt word array:', {promptWords}, 'Correct words:', {correctWords}),
      lastGuess.accuracy = (correctWords.length / promptWords.length) * 100
      checkDone()
    }
  

      // check if game is done

    let gameDone = false

    let checkDone = () => {
 
      if (guessList.length == 6)
      {
        gameDone = true, console.log('Out of tries!')
        }
      else if (lastGuess.accuracy >= 80){
        gameDone = true, console.log('You won!')
      }
      console.log('Game state has been checked.');
      }  

    const styleAccuracy = (answer) => {
    if (answer.accuracy > 80) {
      return {
        color: 'green'
      }
    }
  }

    const answerStyles = (word) => {
      if (allCorrectWords.value.includes(word)) {
      return {
        color: 'green',
        fontFamily: 'space-grotesk'
      }
    }
    else {
      return {
        color: 'black',
        fontFamily: 'space-grotesk'
      }
    }


  }





  
    console.log('User guess:', userGuess, 'Guess list:', guessList )




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

.hint {
  display: inline;
  margin-right: 10px; /* Add some margin for spacing between items */
  background: green;
  width: fit-content;
  color: white;
  font-family: space-grotesk;
}
.inline {
  display: inline;
  margin-right: 10px; /* Add some margin for spacing between items */
  width: fit-content;
}
</style>
