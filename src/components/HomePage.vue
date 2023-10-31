<template>

  <div class="page">
  
  
    <div class="navbar">
      <h1>Hanggman.ai</h1>
      <h4 @onclick="toggleModal">How to play</h4>
    </div>

    <div style="background-color: grey;" @onclick="toggleModal" v-if="modalOpen">
      <div class="modal">
        <p> Hanggman is a simple game where you guess the prompt used to make the ai image!</p>
        <p> The rules are you have 6 attempts</p>
        <p> If you get over 80% over the words right on an attempt, you win. The correct prompt will also be revealed</p>
        <p> If you use all 6 attempts without getting above 80% right on an answer, you lose.</p>
        <p> Every correct word you've found so far will be displayed in the "your correct guesses" segment.</p>
        <p> All your guesses will be displayed on the right. Correct words for that guess will be coloured green. Guess accuracy will also be shown.</p>
        <p> Have fun, challenge a friend, follow me on Twitter!</p>
      </div>
    </div>  
    <div class="playground">

      <div class="timer">
        <h1>NEXT IMAGE IN: 23:43:20...</h1>
      </div>

      <div class="dashboard">
        <div class="img">
          <img src="../assets/walder.png" alt="">          
        </div>


        <div class="guessDiv">

          <div class="guess"> Your right words:
            <div class="guess" id="hint" v-for="word in allCorrectWords" :key="word">
               {{ word }}
            </div>
          </div>
          <div class="guess" id="container" v-for="(answer) in guessList" :key="answer">
              <!--This will display da guesses-->
            <div class="guess">
                <p v-for="word in answer.guessWords" :key="word" :style="answerStyles(word)">
                {{ word }}
              </p>

            </div>
            <span :style="styleAccuracy(answer)"> {{ answer.accuracy }}%</span> 
          </div>
      </div>

    </div>

      <div class="inputGroup" id="input">
        <input class="input" placeholder="Guess the prompt!" @keyup.enter="handleGuess" v-model="userGuessPrime" :disabled="gameDone"/>
        <div class="guess" v-if="gameDone">
          <p>The right prompt was {{ prompt }}</p>
        </div>
      </div> 

  
    </div>

    <div class="navbar" id="footer">
      <p> Follow me! Twitter </p>
    </div>  

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

  let modalOpen = ref(false)
  const toggleModal = () => {modalOpen.value = !modalOpen.value, console.log('get toggled')}
  
    console.log('User guess:', userGuess, 'Guess list:', guessList )


</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

 /* Set bg colour */

body {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  background-color: #204961;
  margin: 0;
}

.page {
  display: flex;
  width: 1440px;
  height: 1024px;
  flex-direction: column;
  align-items: flex-start;
}

/* Navbar */

.navbar {
  display: flex;
  padding: 0px 0px;
  justify-content: space-between;
  align-items: center;
  align-self: stretch;
  height: fit-content;
}

.navbar#footer{
  display: flex;
  justify-content: flex-end;
  height: fit-content
}

/* timer */

.timer {
  display: flex;
  padding: 10px;
  justify-content: center;
  align-items: center;
  gap: 10px;
  align-self: stretch;
  height: fit-content;
}

/* playground */

.playground{
  display: flex;
  flex-direction: column;
  align-items: center;
  align-self: stretch;
  height: 100%;
}
  /* dashboard */

.dashboard{
  display: flex;
  padding: 25px 23px 31px 23px;
  justify-content: center;
  align-items: flex-start;
  align-content: flex-start;
  width: 100%;
  gap: 30px;
  align-self: stretch;
  flex-wrap: nowrap;
  height: 80%;
}

/* img */

.img {
  height: 80%;
  width: 80%;
}

.img img {
  height: 100%;
  width: 100%;
}

      /* guesses */

.guessDiv {
  display: flex;
  padding: 33px 44px 33px 10px;
  flex-direction: column;
  justify-content: flex-start;
  align-items: flex-start;
  gap: 30px;
  width: 90%;
  max-height: 75%;
  overflow: auto;
}

.inputGroup {
  display: flex;
  padding: 27px 106px;
  flex-direction: column;
  align-items: center;
  gap: 37px;
  height: fit-content;
}

.input {
  display: flex;
  padding: 27px 106px;
  flex-direction: column;
  align-items: center;
  gap: 37px;
  border-radius: 11px;
  background: #FFF;
  max-width: 80%;
  height: fit-content;
}

div.guess#hint {
  background: green;
  width: fit-content;
  color: white;
  font-family: space-grotesk;
  justify-content: space-evenly;
}

div.guess#container {
  justify-content: space-between;
  background: white;
  overflow: hidden;
  width: 100%;
}

.guess {
  display: flex;
  padding: 13px 18px;
  justify-content: flex-start;
  align-items: center;
  max-width: 100%;
  width: 100%;
  height: fit-content;
  border-radius: 11px;
  background: #FFF;
  color: black;
  gap: 2%;
  overflow: hidden;
}

.guess p {
  max-width: 100%; /* Set the maximum width for the <p> element */
  word-wrap: break-word; /* Enable word wrap for long words */
}

.promptReveal {
  display: flex;
  padding: 13px 18px;
  justify-content: center;
  align-items: center;
  gap: 204px;
  color: white;
}


/* Modal */

.modal {
  background: white;
}
</style>
