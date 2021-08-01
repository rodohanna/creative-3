<template>
  <section class="hero is-medium is-dark is-bold game-section">
    <div class="hero-body">
      <div class="container response-container">
        <hr />
        <nav class="level is-mobile">
          <div class="level-item has-text-centered">
            <div>
              <p class="heading">High Score</p>
              <p class="title">{{ highScore }}</p>
            </div>
          </div>
          <div class="level-item has-text-centered">
            <div>
              <p class="heading">Difficulty</p>
              <p class="title">{{ difficultyFormatted }}</p>
            </div>
          </div>
        </nav>
        <hr />
        <div class="level-data">
          <span class="stats">
            <i class="fas fa-star positive-points"></i>
            <span id="points">{{ correctQuestions }}</span>
          </span>
          <span class="timer">
            <i class="far fa-clock"></i>
            <span id="timer-text">{{ time }}</span>
          </span>
        </div>
        <h2 class="subtitle">"{{ question }}"</h2>
        <a
          class="button is-medium is-light is-fullwidth is-rounded question-button"
          v-for="button in buttons"
          v-bind:key="button"
          v-on:click="handleButtonClick"
          >{{ button }}</a
        >
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
    "handleStateChange",
    "incrementCorrectQuestions",
    "highScore",
    "correctQuestions",
  ],
  created() {
    this.startTimer();
  },
  data() {
    return {
      time: 30,
      block: false,
      interval: null,
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
            (button) => {
              if (button.textContent === appContext.correctAnswer) {
                button.classList.add("is-primary");
                button.classList.remove("is-danger");
              }
            }
          );
          setTimeout(() => {
            appContext.handleStateChange("question");
          }, 1500);
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
        this.incrementCorrectQuestions();
        e.target.classList.add("is-primary");
      } else {
        e.target.classList.add("is-danger");
        Array.from(document.querySelectorAll(".question-button")).forEach(
          (button) => {
            if (button.textContent === this.correctAnswer) {
              button.classList.add("is-primary");
              button.classList.remove("is-danger");
            }
          }
        );
      }
      const appContext = this;
      setTimeout(() => {
        appContext.handleStateChange("question");
      }, 1500);
    },
  },
  computed: {
    difficultyFormatted: function() {
      return this.difficulty.charAt(0).toUpperCase() + this.difficulty.slice(1);
    },
  },
  watch: {
    question: function() {
      this.block = false;
      this.time = 30;
      Array.from(document.querySelectorAll(".question-button")).forEach(
        (button) => {
          button.classList.remove("is-primary");
          button.classList.remove("is-danger");
        }
      );
      this.startTimer();
    },
  },
};
</script>

<style scoped>
.positive-points {
  color: #ffdc00;
  margin: 4px;
}

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
  height: 50px;
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
