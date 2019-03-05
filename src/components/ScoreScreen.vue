<template>
  <section class="hero is-fullheight is-dark is-bold results-section hide">
    <div class="hero-body">
      <div class="container result-container">
        <h1 class="title banner-title results-title">
          You Got
          <span class="points-color">{{correctQuestions}}</span>
          {{questionsText}} Correct
        </h1>
        <h2 class="subtitle results-subtitle">{{feedbackText}}</h2>
        <a class="button is-info try-again-btn" v-on:click="handleTryAgain">Try Again</a>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  props: ["correctQuestions", "resetGame"],
  computed: {
    questionsText: function() {
      return this.correctQuestions === 1 ? "Scripture" : "Scriptures";
    },
    feedbackText: function() {
      if (this.correctQuestions === 0) {
        return "No worries, try again!";
      }
      if (this.correctQuestions < 3) {
        return "Not too bad!";
      }
      if (this.correctQuestions < 6) {
        return "Wow, you're pretty good!";
      }
      if (this.correctQuestions === 10) {
        return "Unbelievable. Bravo.";
      }
      return "Fantastic!";
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
