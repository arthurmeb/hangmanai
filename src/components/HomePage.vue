<template>

  <div class="page">
  
    <div class="navbar">
      <h1>Hanggman</h1>
      <p @click="toggleModal">How to play</p>
    </div>

    <div class="backdrop" @click.self="toggleModal" v-if="modalOpen">
      <div class="modal" v-if="modalOpen">
        <div class="modal" id="container"> 
          <h1> How to play </h1>
          <p> Guess the prompt in 6 attempts.</p>
          <p> Each attempt will show your correct words and accuracy.</p>
          <img src="../assets/modal1.png" alt="" class="modalImg">
          <p> All your correct words so far will be displayed. </p>
          <img src="../assets/modal2.png" alt="" class="modalImg">
          <p> The prompt will be revealed when the game is done.</p>
          <img src="../assets/modal3.png" alt="" class="modalImg">
          <p> Score over 80% on any guess to win </p>
          <p> Have fun, challenge a friend, follow me on Twitter!</p>
          <button @click="toggleModal"> I understand! </button>          
        </div>
      </div>
    </div>  

    <div class="playground">

      <div class="timer">
        <h1>NEXT IMAGE IN: {{ countdown }}</h1>
      </div>

      <div class="dashboard">
        <div class="image">
          <img src="../assets/walder.png" alt="">     
          <input class="input" placeholder="Guess the prompt!" @keyup.enter="handleGuess" v-model="userGuessPrime" :disabled="gameDone"/>          
        </div>


        <div class="guessDiv">

          <div class="guess"> <p> Your correct words:</p>
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

      <div v-if="won" style="background: green;"> You won, good job. Treat yourself to a cookie...</div>
      <div v-if="lost" style="background: red;"> Uh oh, you hanged the man! Better luck in {{countdown}}...</div>

      <div class="input" id="promptReveal" v-if="gameDone">
        <p>The right prompt was: <span style="color:rgb(54, 224, 61)">{{ prompt }}</span></p>
      </div>

    </div> 
  
    </div>

    <div class="navbar" id="footer">
      <p> Follow me! Twitter </p>
    </div>  

  </div>
  </template>
  
  
  <script setup>
  import { computed, onMounted} from "vue";
  import { ref } from "vue"

    let modalOpen = ref(false)
    let toggleModal = () => {modalOpen.value = !modalOpen.value, console.log('get toggled', modalOpen)}


    const prompt = 'Walter White in shades smoking a cigarette'.toLowerCase()
  
  
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
    let won = false
    let lost = false

    let checkDone = () => {
 
      if (guessList.length == 6)
      {
        gameDone = true
        lost = true
        }
      else if (lastGuess.accuracy >= 80){
        gameDone = true
        won = true
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
        color: '#36E03D',
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
 
const countdown = ref('');
const updateCountdown = () => {
  const now = new Date();
  const targetTime = new Date();

  // Set the target time to midnight GMT+0
  targetTime.setUTCHours(24, 0, 0, 0);

  // Calculate the time difference (in milliseconds) between now and midnight GMT+0
  const timeDiff = targetTime - now;

  // Calculate hours, minutes, and seconds from the time difference
  const hours = Math.floor(timeDiff / (1000 * 60 * 60));
  const minutes = Math.floor((timeDiff / (1000 * 60)) % 60);
  const seconds = Math.floor((timeDiff / 1000) % 60);

  // Format the countdown string
  countdown.value = `${hours} hours ${minutes} minutes ${seconds} seconds`;
};

// Call updateCountdown when the component is mounted
onMounted(() => {
  updateCountdown();
  // Update the countdown every second
  setInterval(updateCountdown, 1000);
});


</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

 /* Set bg colour */

body {
  display: flex;
  flex-direction: column;
  align-items: stretch;
  background-color: #282424;
  margin: 0;
  width: 100vw;
  height: 100vh;
  overflow-x: hidden;
}

.page {
  display: flex;
  flex: 1;
  width: 100vw;
  height: 100vh;
  flex-direction: column;
  align-items: flex-start;
  overflow-x: hidden;
}

/* Navbar and footer */

.navbar {
  display: flex;
  flex-direction: row;
  padding: 0px 5vw;
  justify-content: space-between;
  align-items: center;
  align-self: stretch;
  height: fit-content;
  font-family: 'space-grotesk';
  font-weight: bold;
}

.navbar p {
  font-family: 'space-grotesk';
  font-weight: lighter;
}

.navbar#footer{
  display: flex;
  justify-content: flex-end;
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
  border: 1px solid white;
  margin: 0.2rem;
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
  padding: 1vw 0vw;
  justify-content: space-evenly;
  align-items: flex-start;
  align-content: flex-start;
  width: 100%;
  gap: 30px;
  align-self: stretch;
  flex-wrap: nowrap;
  height: 80%;
}

/* img */

.image {
  height: fit-content;
  width: fit-content;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2vh;

}

.image img {
  height: fit-content;
  width: 40vw;
}

      /* guesses */

.guessDiv {
  display: flex;
  padding: 2px 2px 0px 2px;
  flex-direction: column;
  justify-content: flex-start;
  align-items: flex-start;
  gap: 2vw;
  width: 90%;
  height: 80%;
  overflow: auto;
}

div.guess#hint {
  background: green;
  width: fit-content;
  color: white;
  font-family: space-grotesk;
  justify-content: space-evenly;
  margin-bottom: 1%;
}

div.guess#container {
  justify-content: space-between;
  background: white;
  overflow: visible;
  width: 90%;
  flex-wrap: nowrap;
  min-height: fit-content;
}

.guess {
  display: flex;
  padding: 0 1vw;
  justify-content: flex-start;
  align-items: center;
  width: 90%;
  border-radius: 11px;
  background: #FFF;
  color: black;
  gap: 1%;
  overflow: visible;
  flex-wrap: wrap;
  min-height: fit-content;
}

.guess p {
  max-width: 100%; /* Set the maximum width for the <p> element */
  word-wrap: break-word; /* Enable word wrap for long words */
}

/* Input and reveal */

.inputGroup {
  display: flex;
  padding: 27px 0px;
  flex-direction: column;
  align-items: center;
  gap: 37px;
  height: fit-content;
  width: 100%;
  font-family: space-grotesk;
}

.input {
  display: flex;
  padding: 13px 18px;
  align-items: center;
  gap: 10px;
  border-radius: 11px;
  height: fit-content;
  width: 80%;
  font-family: space-grotesk;
}

.input#promptReveal {
  width: 40%;
  background: white;
  color: black;
  font: inherit;
  justify-content: center;
}


/* Modal */

.backdrop {
  display: block; /* Hidden by default */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0,0,0); /* Fallback color */
  background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
}

/* Modal Content/Box */
.modal {
  background-color: #fefefe;
  display: flex;
  flex-direction: column;
  margin: 5% auto; /* 15% from the top and centered */
  padding: 20px;
  border-radius: 11px;
  width: fit-content; /* Could be more or less, depending on screen size */
  color: black;
  align-items: center;
}

.modal#container {
  width: 30rem;
}

.modal button {
  border-radius: 11px;
  background-color: green;
  color: white;
  border: transparent;
  font-family: space-grotesk;
}

.modalImg {
  height: 100%;
  width: 90%;
}

.modal p{
  width:fit-content;
}
/* scrollbar */

.container {
  display: flex;
  margin: 50px;
  column-gap: 20px;
}

.scrollbar {
  height: 300px;
  width: 50%;
  overflow: auto;
  padding: 0 10px;
}

.guessDiv::-webkit-scrollbar {
  width: 12px;
}
        
.guessDiv::-webkit-scrollbar-track {
  border-radius: 8px;
  background-color: #95a5a6;
  border: 1px solid #cacaca;
}

.guessDiv::-webkit-scrollbar-thumb {
  border-radius: 8px;
  background-color: yellow;
}

/* media queries */

@media (max-width: 426px) {
  .dashboard {
    flex-direction: column;
    align-items: center;
  }

  .navbar {
    font-size:x-small
  }

  .image img {
    width: 80vw;
  }

  .modal#container{
    width: 70vw;
    height: fit-content;
    font-size: small;
  }

}

@media (min-width: 2440px) {
  .image img {
    width: 50vw;
  }

  .modal#container{
    width: 50vw;
    height: 70vw;
    font-size: 2vw;
  }


  
}
</style>
