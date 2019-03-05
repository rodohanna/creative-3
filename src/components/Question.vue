<template>
  <section class="hero is-medium is-info is-bold game-section hide">
    <div class="hero-body">
      <div class="container response-container">
        <hr>
        <nav class="level is-mobile">
          <div class="level-item has-text-centered">
            <div>
              <p class="heading">High Score</p>
              <p class="title">{{streak}}</p>
            </div>
          </div>
          <div class="level-item has-text-centered">
            <div>
              <p class="heading">Difficulty</p>
              <p class="title">{{difficulty}}</p>
            </div>
          </div>
        </nav>
        <hr>
        <div class="level-data">
          <span class="stats">
            <i class="fas fa-star"></i>
            <span id="points">{{points}}</span>
          </span>
          <span class="timer">
            <i class="far fa-clock"></i>
            <span id="timer-text">{{time}}</span>
          </span>
        </div>
        <h2 class="subtitle">{{question}}</h2>
        <a
          class="button is-medium is-light is-fullwidth is-rounded question-button"
          v-for="button in buttons"
          v-bind:key="button"
        >{{button}}</a>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  props: ["buttons", "streak", "difficulty", "question", "points"],
  data() {
    return {
      time: 10
    };
  }
};
</script>

<style scoped>
.button {
  margin-top: 20px;
  height: 75px;
  white-space: unset;
  text-align: center;
}

.stats {
  float: left;
}

.timer {
  float: right;
  min-width: 60px;
  display: flex;
  justify-content: space-between;
}

.level-data {
  width: 100%;
  display: block;
  margin-bottom: 30px;
  height: 30px;
  font-size: 25px;
}

.fa-clock {
  display: flex;
  align-items: center;
}

.response-container {
  text-align: center;
}

.response-container .button {
  margin-top: 20px;
  height: 75px;
  white-space: unset;
  text-align: center;
}

.response-container .subtitle {
  font-size: 28px;
}

@media only screen and (min-width: 1000px) {
  .response-container {
    width: 50%;
    min-width: 400px;
  }
}
</style>

