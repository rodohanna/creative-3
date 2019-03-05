<template>
  <section class="hero is-fullheight is-info is-bold main-banner">
    <div class="hero-body">
      <div class="container banner-container">
        <h1 class="title banner-title">40 Seconds | 10 Questions</h1>
        <h2 class="subtitle">
          You get 40 seconds to answer 10 trivia questions.. How many can you
          get correct?
        </h2>
        <div class="columns">
          <div class="column is-6">
            <a class="button is-primary start-game-btn" v-on:click="handleBegin">BEGIN</a>
          </div>
          <div class="column is-6">
            <div class="select">
              <select id="select-difficulty">
                <option value="easy">Easy</option>
                <option value="medium">Medium</option>
                <option value="hard">Hard</option>
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
  props: ["handleStateChange"],
  methods: {
    handleBegin: function() {
      this.handleStateChange("question");
    }
  }
};
</script>
