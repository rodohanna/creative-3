<template>
  <section class="hero is-fullheight is-info is-bold main-banner">
    <div class="hero-body">
      <div class="container banner-container">
        <h1 class="title banner-title">40 Seconds | 10 Questions</h1>
        <h2 class="subtitle">
          You get 40 seconds to answer 10 trivia questions.. How many can you
          get correct?
        </h2>
        <div class="columns is-mobile">
          <div class="column is-6">
            <a class="button is-primary start-game-btn" v-on:click="handleBegin">BEGIN</a>
          </div>
          <div class="column is-6">
            <div class="select">
              <select id="select-difficulty" @change="onDifficultyChange($event)">
                <option value="easy">Easy</option>
                <option value="medium">Medium</option>
                <option value="hard">Hard</option>
                <option value="prophet">Prophet</option>
              </select>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  props: ["handleStateChange", "handleDifficultyChange"],
  methods: {
    handleBegin: function() {
      this.handleStateChange("question");
    },
    onDifficultyChange: function() {
      this.handleDifficultyChange(event.target.value);
    }
  }
};
</script>

<style scoped>
.start-game-btn,
.select {
  font-size: 25px;
}

.select,
#select-difficulty {
  width: 100%;
}

@media only screen and (min-width: 1000px) {
  .columns {
    width: 75%;
    margin: 0 auto;
  }
}

@media only screen and (max-width: 860px) {
  .select,
  .start-game-btn {
    font-size: 22px;
  }
}

@media only screen and (max-width: 760px) {
  .banner-title {
    font-size: 40px;
  }
  .select,
  .start-game-btn {
    font-size: 18px;
  }
}

@media only screen and (max-width: 650px) {
  .banner-title {
    font-size: 30px;
  }
  .select,
  .start-game-btn {
    font-size: 12px;
  }
}
</style>
