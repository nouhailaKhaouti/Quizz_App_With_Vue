<template>
    <div>
              <h1 id="question">{{ currentQuestion.question }}</h1>
            <div id="reponse" class="reponse">
              <button :style="{ backgroundColor: buttonColor }" v-for="(answer, index) in currentQuestion.shuffledAnswers"
                :key="index" @click="selectAnswer(answer)">
                {{ answer.answer }}
              </button>
            </div>
    </div>
</template>

<script>
export default {
    methods: {
    selectAnswer(answer) {
      if (answer.correct) {
        this.setStatusClass(answer, answer.correct);
        this.score++;
      }
      console.log(this.shuffledQuestions);
      this.shuffledColors = this.color.sort(() => Math.random() - 0.5);
      const nextIndex = this.shuffledQuestions.indexOf(this.currentQuestion) + 1;
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
}
}
</script>

<style>

</style>