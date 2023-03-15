<template>
  <div>
    <div class="pro">
      <label for="file" id="progressName" value="50">{{ progresse }}%</label>
      <div id="progresser"><progress id="progress" :value="progresse" max="100"> </progress></div>
      <img src="./assets/php-g26a9a3b39_1920-removebg-preview.png" alt="logo" width="150" height="100">
      <div class="score" id="score">''</div>
    </div>
    <div class="main">
      <div class="best" id="best" style="color: white;">
        <!-- <h1>{{ quizTitle }}</h1> -->
        <div v-if="!quizStarted&&!quizCount">
          <pre>
                1- You will have only <span>30 seconds</span> per each question.

                  2- Once you select your answer, you can't reselect.

                 3- You can't select any option once time goes off.

                    4- You can't exit from the Quiz while you're playing.

                         5- You'll get points on the basis of your correct answers.
                        </pre>
        </div>
        <div class="timer" id="timer" v-if="quizCount && !quizStarted">{{ timer }}</div>
        <div v-if="quizStarted">
          <div id="container" class="container" v-if="!quizFinished">
            <h1 id="question">{{ currentQuestion.question }}</h1>
            <div id="reponse" class="reponse">
              <button type="button" class="btn" :style="{ backgroundColor: buttonColor }" v-for="(answer, index) in currentQuestion.answers"
                :key="index" @click="selectAnswer(answer)">
                {{ answer.answer }}
              </button>
            </div>
          </div>
          <div v-else>
            <div class="star hide" id="star">
              <div class="filler" id="filler"> </div>
              <div class="flex">
                <img src="./assets/star.png" alt="star" class="img" width="80" height="70">
                <img src="./assets/star.png" alt="star" class="img" width="80" height="70">
                <img src="./assets/star.png" alt="star" class="img" width="80" height="70">
                <img src="./assets/star.png" alt="star" class="img" width="80" height="70">
                <img src="./assets/star.png" alt="star" class="img" width="80" height="70">
              </div>
            </div>
            <h2>Congratulations! You finished the quiz.</h2>
            <h3>Your score: {{ score }}/{{ questions.length }}</h3>
            <p :style="{ backgroundColor: buttonColor }" v-for="(wrongquestion, index) in wrongquestion"
                :key="index" >
                {{ wrongquestion.question }}
                <br>
                ****************************
                <br>
                {{ wrongquestion.answer.answer}}
                <br>
            </p>
          </div>
        </div>
        <br>            
        <div class="controls">
          <button id="start" class="start-btn btn " style="background-color: black;" v-if="!quizCount"
            @click="startTimer()">Start Quiz</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      quizTitle: 'PHP Quizz',
      timer: 5,
      questions: [
        {
          question: "What is PHP?",
          answers: [
            { answer: "PHP is an open-source programming language", correct: true },
            { answer: " PHP is a server-side scripting language", correct: false },
            { answer: " PHP is a server-side scripting language", correct: false },
          ],
        },
        {
          question: "Who is the father of PHP ?",
          answers: [
            { answer: " Drek Kolkevi", correct: true },
            { answer: "List Barely", correct: false },
            { answer: " Willam Makepiece", correct: false },
          ],
        },
        {
          question:
            "Which of the following is the correct syntax to write a PHP code?",
          answers: [
            { answer: "'<?php ?>'", correct: true },
            { answer: "'< php >'", correct: false },
            { answer: "'< ? php ?>'", correct: false },
          ],
        },
        {
          question:
            " Which of the following is the correct way to add a comment in PHP code?",
          answers: [
            { answer: " '#'", correct: true },
            { answer: "'//'", correct: false },
            { answer: "'/* */'", correct: false },
          ],
        },
      ],
      color: [
        {
          colors: ["rgb(158, 249, 228)", "rgb(158, 149, 228)", "rgb(158, 49, 228)"]
        },
        {
          colors: ["rgb(242, 211, 255)", "rgb(242, 111, 255)", "rgb(242, 11, 255)"]
        },
        {
          colors: ["rgb(149, 155, 214)", "rgb(115, 103, 248)", "rgb(57, 57, 228)"]
        },
        {
          colors: ["rgb(151, 184, 232)", "rgb(116, 168, 247)", "rgb(73, 146, 255)"]
        },
      ],
      currentQuestion: null,
      wrongquestion:[],
      progresse:0,
      correct:null,
      buttonColor: null,
      shuffledQuestions: null,
      quizStarted: false,
      quizFinished: false,
      quizCount: false,
      score: 0,
      colorIndex: 0,
    }
  },
  methods: {
    startQuiz() {
      this.shuffledColors = this.color.sort(() => Math.random() - 0.5);
      this.shuffledQuestions = this.questions.sort(() => Math.random() - 0.5);
      this.currentQuestion = this.shuffledQuestions[0];
      this.shuffledAnswers = this.currentQuestion.answers.sort(() => Math.random() - 0.5);
      console.log(this.currentQuestion.question);
      console.log(this.currentQuestion.answers);
      this.buttonColor = `radial-gradient(circle at center,${this.shuffledColors[this.colorIndex].colors[0]},${this.shuffledColors[this.colorIndex].colors[1]},${this.shuffledColors[this.colorIndex].colors[2]}),100%`;
      this.quizStarted = true;
      this.quizFinished = false;
      this.score = 0;
    },
    setStatusClass(element, correct) {
      if (correct) {
        this.buttonColor = "#B6FFCE";
      } else {
        element.style.background = "#FF8C8C";
      }
    },
    selectAnswer(answer) {
      // this.setStatusClass(answer, answer.correct);
      if (answer.correct) {
        this.score++;
      }else{
        this.correctAnswer = this.currentQuestion.answers.find((answer) => answer.correct);
        this.wrongquestion.push({question:this.currentQuestion.question,answer:this.correctAnswer});
      }
      console.log(this.shuffledQuestions);
      this.shuffledColors = this.color.sort(() => Math.random() - 0.5);
      const nextIndex = this.shuffledQuestions.indexOf(this.currentQuestion) + 1;
      this.progresse= Math.trunc((nextIndex * 100) / (this.questions.length - 1));
      if (nextIndex < this.shuffledQuestions.length) {
        this.currentQuestion = this.shuffledQuestions[nextIndex];
        this.shuffledAnswers = this.currentQuestion.answers.sort(() => Math.random() - 0.5);
        this.buttonColor = `radial-gradient(circle at center,${this.shuffledColors[this.colorIndex].colors[0]},${this.shuffledColors[this.colorIndex].colors[1]},${this.shuffledColors[this.colorIndex].colors[2]}),100%`;
        this.colorIndex++;
      } else {
        this.quizCount = false;
        this.quizFinished = true;
      }
    },
    startTimer() {
      this.quizCount = true;
      let counter = 5;

      const interval = setInterval(() => {
        counter--;
        this.timer = counter;
        if (counter == 0) {
          clearInterval(interval);
          this.startQuiz();
        }
      }, 1000);
    }

  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

@import url("https://fonts.googleapis.com/css2?family=Catamaran&display=swap");

@-webkit-keyframes zoom-in-zoom-out {
  0% {
    transform: scale(3.2, 3.2);
  }

  100% {
    transform: scale(4.2, 4.2);
  }
}

@keyframes zoom-in-zoom-out {
  0% {
    transform: scale(3.2, 3.2);
  }

  100% {
    transform: scale(4.2, 4.2);
  }
}

@-webkit-keyframes flicker {

  0%,
  18%,
  22%,
  25%,
  53%,
  57%,
  100% {
    text-shadow: 0 0 7px #fff, 0 0 10px #fff, 0 0 21px rgb(253, 200, 253), 0 0 42px rgb(232, 132, 241), 0 0 82px rgb(255, 0, 217), 0 0 92px rgb(255, 0, 162), 0 0 102px rgb(255, 0, 140), 0 0 151px rgb(255, 0, 106);
  }

  20%,
  24%,
  55% {
    text-shadow: none;
  }
}

@keyframes flicker {

  0%,
  18%,
  22%,
  25%,
  53%,
  57%,
  100% {
    text-shadow: 0 0 7px #fff, 0 0 10px #fff, 0 0 21px rgb(253, 200, 253), 0 0 42px rgb(232, 132, 241), 0 0 82px rgb(255, 0, 217), 0 0 92px rgb(255, 0, 162), 0 0 102px rgb(255, 0, 140), 0 0 151px rgb(255, 0, 106);
  }

  20%,
  24%,
  55% {
    text-shadow: none;
  }
}

span {
  -webkit-animation: flicker 1s infinite alternate;
  animation: flicker 1s infinite alternate;
  color: #fff;
}

* {
  font-family: "Catamaran", sans-serif;
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  text-align: center;
}

*:before,
*:after {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

.background {
  width: 430px;
  height: 500px;
  position: absolute;
  transform: translate(-50%, -50%);
  left: 50%;
  top: 50%;
}

.background .shape {
  height: 200px;
  width: 200px;
  position: absolute;
  border-radius: 50%;
}

.background .shape:first-child {
  background: linear-gradient(rgb(252, 10, 167), rgb(149, 3, 247));
  left: -80px;
  top: -80px;
}

.background .shape:last-child {
  background: linear-gradient(to right, #2f82ff, #6edef7);
  right: -30px;
  bottom: -80px;
}
.timer {
  margin-top: 4%;
  margin-bottom: 4%;
  -webkit-animation: zoom-in-zoom-out 1s ease infinite;
  animation: zoom-in-zoom-out 1s ease infinite;
  color: #ffffff;
  text-shadow: 0 0 7px #fff, 0 0 10px #fff, 0 0 21px rgb(253, 200, 253), 0 0 42px rgb(232, 132, 241), 0 0 82px rgb(255, 0, 217), 0 0 92px rgb(255, 0, 162), 0 0 102px rgb(255, 0, 140), 0 0 151px rgb(255, 0, 106);
  font-size: 3.2rem;
}

body {
  background: url(./assets/5053309.jpg) repeat;
  background-size: 25%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.correct {
  background-color: rgb(133, 241, 133);
}

.wrong {
  background-color: rgb(255, 112, 112);
}

.main {
  width: 80%;
  height: 60%;
  margin: auto;
  border-radius: 10px;
  box-shadow: 0px 4px 9px 0px rgba(86, 185, 235, 0.5), 1px -6px 8px rgba(86, 185, 235, 0.5);
  text-align: center;
  top: 20%;
}

.controls {
  display: flex;
  justify-content: center;
}

img {
  margin-bottom: 5%;
}

.hide {
  display: none;
}

#question {
  margin-top: 5%;
  margin-left: 10%;
  color: aliceblue;
  box-shadow: 0px 4px 9px 0px rgba(86, 185, 235, 0.5), 1px -6px 8px rgba(86, 185, 235, 0.5);
  width: 80%;
  border-radius: 7px;
}

.pro {
  display: flex;
  margin-top: 2%;
}

.reponse {
  margin-top: 8%;
  display: flex;
  justify-content: space-around;
}

#progresser {
  padding-top: 8%;
}

progress[value] {
  /* Reset the default appearance */
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  width: 450px;
  height: 20px;
}

progress[value]::-webkit-progress-bar {
  background-color: rgb(40, 37, 37);
  border-radius: 10px;
  box-shadow: 0 2px 5px rgba(112, 110, 110, 0.25) inset;
}

progress[value]::-webkit-progress-value {
  background-image: -webkit-linear-gradient(-45deg, transparent 33%, rgba(0, 0, 0, 0.1) 33%, rgba(0, 0, 0, 0.1) 66%, transparent 66%), -webkit-linear-gradient(left, rgb(149, 3, 247), rgb(252, 10, 167));
  border-radius: 10px;
  background-size: 35px 20px, 100% 100%, 100% 100%;
}

.btn,
.next-btn,
.start-btn {
  box-shadow: 0 0.2rem 0.6rem 0 rgba(242, 247, 250, 0.5);
  border-radius: 0.5em;
  border-color: rgba(240, 255, 255, 0.306);
  color: rgb(253, 252, 255);
  padding: 0px 10px;
  font-size: large;
  width: 150px;
  height: 150px;
  margin: 0 10px;
  background:black ;
  text-shadow: 0 0 7px #fff, 0 0 10px #fff, 0 0 21px rgb(253, 200, 253), 0 0 42px rgb(232, 132, 241), 0 0 82px rgb(255, 0, 217), 0 0 92px rgb(255, 0, 162), 0 0 102px rgb(255, 0, 140), 0 0 151px rgb(255, 0, 106);
  box-shadow: 0 0 0.2rem #fff, 0 0 0.2rem #fff, 0 0 2rem #bc13fe, 0 0 0.8rem #bc13fe, 0 0 2.8rem #bc13fe, inset 0 0 1.3rem #bc13fe;
}

.start-btn {
  width: 20%;
  height: 20%;
  margin: 10px 10px;
  text-align: center;
  -webkit-backdrop-filter: blur(10px);
  backdrop-filter: blur(10px);
  text-shadow: 0 0 7px #fff, 0 0 10px #fff, 0 0 21px rgb(253, 200, 253), 0 0 42px rgb(232, 132, 241), 0 0 82px rgb(255, 0, 217), 0 0 92px rgb(255, 0, 162), 0 0 102px rgb(255, 0, 140), 0 0 151px rgb(255, 0, 106);
  box-shadow: 0 0 0.2rem #fff, 0 0 0.2rem #fff, 0 0 2rem #bc13fe, 0 0 0.8rem #bc13fe, 0 0 2.8rem #bc13fe, inset 0 0 1.3rem #bc13fe;
}

.filler {
  background-color: rgb(223, 223, 16);
  position: absolute;
  height: 60px;
  left: 34%;
  justify-content: center;
  margin-top: 13px;
}

.flex {
  display: flex;
  justify-content: center;
  position: absolute;
  left: 34%;
}

.star {
  margin-top: 2%;
  border-style: solid;
  border-radius: 30px;
  border-color: rgba(250, 250, 250, 0.53);
  width: 450px;
  height: 90px;
  background-color: black;
  margin-left: 28%;
}

.img {
  margin-top: 10px;
  display: block;
  border-collapse: collapse;
  border-spacing: 0;
}

button:hover {
  box-shadow: 0 0.4rem 1.4rem 0 rgba(86, 185, 235, 0.5);
  transform: translateY(-0.5rem);
  transition: transform 150ms;
}

.container {
  width: 100%;
  height: 100%;
}

/*# sourceMappingURL=style.css.map */
</style>
