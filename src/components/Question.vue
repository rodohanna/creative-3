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
              <p class="title">{{difficultyFormatted}}</p>
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
        <h2 class="subtitle">"{{question}}"</h2>
        <a
          class="button is-medium is-light is-fullwidth is-rounded question-button"
          v-for="button in buttons"
          v-bind:key="button"
          v-on:click="handleButtonClick"
        >{{button}}</a>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  props: [
    "buttons",
    "streak",
    "difficulty",
    "question",
    "points",
    "correctAnswer",
    "handleStateChange"
  ],
  created() {
    this.startTimer();
  },
  data() {
    return {
      time: 10,
      block: false,
      interval: null
    };
  },
  methods: {
    startTimer: function() {
      const appContext = this;
      clearInterval(this.interval);
      this.interval = setInterval(() => {
        appContext.time -= 1;
        if (appContext.time < 1) {
          appContext.block = true;
          clearInterval(appContext.interval);
          Array.from(document.querySelectorAll(".question-button")).forEach(
            button => {
              if (button.textContent === appContext.correctAnswer) {
                button.classList.add("is-primary");
                button.classList.remove("is-danger");
              }
            }
          );
          setTimeout(() => {
            appContext.handleStateChange("question");
          }, 1000);
        }
      }, 1000);
    },
    handleButtonClick: function(e) {
      if (this.block) {
        return;
      }
      clearInterval(this.interval);
      this.block = true;
      const isCorrectAnswer = e.target.textContent === this.correctAnswer;
      if (isCorrectAnswer) {
        e.target.classList.add("is-primary");
      } else {
        e.target.classList.add("is-danger");
      }
      const appContext = this;
      setTimeout(() => {
        appContext.handleStateChange("question");
      }, 2000);
    }
  },
  computed: {
    difficultyFormatted: function() {
      return this.difficulty.charAt(0).toUpperCase() + this.difficulty.slice(1);
    }
  },
  watch: {
    question: function() {
      this.block = false;
      this.time = 10;
      Array.from(document.querySelectorAll(".question-button")).forEach(
        button => {
          button.classList.remove("is-primary");
          button.classList.remove("is-danger");
        }
      );
      this.startTimer();
    }
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

