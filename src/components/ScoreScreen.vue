<template>
  <section class="hero is-fullheight is-info is-bold results-section hide">
    <div class="hero-body">
      <div class="container result-container">
        <h1 class="title banner-title results-title">
          You Got
          <span class="points-color">{{correctQuestions}}</span>
          {{questionsText}} Correct
        </h1>
        <h2 class="subtitle results-subtitle">Great Job!</h2>
        <a class="button is-primary try-again-btn" v-on:click="handleTryAgain">Try Again</a>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  props: ["correctQuestions", "resetGame"],
  computed: {
    questionsText: function() {
      return this.correctQuestions === 1 ? "Question" : "Questions";
    }
  },
  methods: {
    handleTryAgain: function() {
      this.resetGame();
    }
  }
};
</script>


<style scoped>
.results-subtitle {
  font-size: 30px;
}
.points-color {
  color: #111111;
  font-size: 75px;
}
.banner-title {
  font-size: 50px;
}
.try-again-btn {
  font-size: 30px;
}
@media only screen and (max-width: 760px) {
  .banner-title {
    font-size: 40px;
  }
}
@media only screen and (max-width: 650px) {
  .banner-title {
    font-size: 30px;
  }
}
</style>
