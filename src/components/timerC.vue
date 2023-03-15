<template>
          <div class="timer hide" id="timer" v-if="quizCount && !quizStarted">{{ timer }}</div>
</template>

<script>
export default {
    methods: {
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

</style>