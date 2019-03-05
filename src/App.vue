<template>
  <div id="app">
    <MainBanner
      v-if="state == 'begin'"
      v-bind:handleStateChange="handleStateChange"
      v-bind:handleDifficultyChange="handleDifficultyChange"
    />
    <ScoreScreen
      v-if="state == 'score'"
      v-bind:correctQuestions="correctQuestions"
      v-bind:resetGame="resetGame"
    />
    <Question
      v-if="state == 'question' && bomLoaded === true"
      v-bind:buttons="books"
      v-bind:correctAnswer="correctBook"
      v-bind:difficulty="difficultyLevel"
      v-bind:question="questionText"
      v-bind:highScore="highScore"
      v-bind:correctQuestions="correctQuestions"
      v-bind:handleStateChange="handleStateChange"
      v-bind:incrementCorrectQuestions="incrementCorrectQuestions"
    />
    <Loading v-if="state == 'question' && bomLoaded === false"/>
    <Footer/>
  </div>
</template>

<script>
import MainBanner from "./components/MainBanner.vue";
import ScoreScreen from "./components/ScoreScreen.vue";
import Question from "./components/Question.vue";
import Loading from "./components/Loading.vue";
import Footer from "./components/Footer.vue";
import { setTimeout } from "timers";

export default {
  name: "app",
  components: {
    MainBanner,
    ScoreScreen,
    Question,
    Loading,
    Footer
  },
  async created() {
    this.bom = await fetch("/bom.json").then(r => r.json());
    this.bomLoaded = true;
  },
  data() {
    return {
      bomLoaded: false,
      currQuestion: 0,
      numQuestions: 10,
      highScore: 0,
      correctQuestions: 0,
      state: "begin",
      bom: null,
      gameIsRunning: false,
      questionText: "",
      books: [],
      correctBook: "",
      difficultyLevel: "easy"
    };
  },
  methods: {
    incrementCorrectQuestions: function() {
      this.correctQuestions += 1;
    },
    handleStateChange: async function(newState) {
      console.log({ newState });
      this.state = newState;
      if (this.state === "question") {
        console.log("start game");
        while (this.bomLoaded === false) {
          await new Promise(r => setTimeout(r, 100));
        }
        this.gameLoop();
      }
    },
    handleDifficultyChange: function(difficulty) {
      this.difficultyLevel = difficulty;
    },
    gameLoop: function() {
      this.currQuestion += 1;
      if (this.currQuestion >= this.numQuestions + 1) {
        this.handleStateChange("score");
        return;
      }
      /**
       * Randomly shuffle an array
       * https://stackoverflow.com/a/2450976/1293256
       * @param  {Array} array The array to shuffle
       * @return {String}      The first item in the shuffled array
       */
      var shuffle = function(array) {
        var currentIndex = array.length;
        var temporaryValue, randomIndex;

        // While there remain elements to shuffle...
        while (0 !== currentIndex) {
          // Pick a remaining element...
          randomIndex = Math.floor(Math.random() * currentIndex);
          currentIndex -= 1;

          // And swap it with the current element.
          temporaryValue = array[currentIndex];
          array[currentIndex] = array[randomIndex];
          array[randomIndex] = temporaryValue;
        }

        return array;
      };
      const randRange = (min, max) => {
        return parseInt(Math.random() * (max - min) + min);
      };
      const books = this.bom.books;
      const bookNames = books.map(book => book.book.toLowerCase()).join("\n");
      const randBook = books[randRange(0, books.length - 1)];
      const chapters = randBook.chapters;
      const randChapter = chapters[randRange(0, chapters.length - 1)];
      const verses = randChapter.verses;
      const randVerse = verses[randRange(0, verses.length - 1)];

      this.correctBook = randBook.book;

      let spliceNum = 14;
      if (this.difficultyLevel === "easy") {
        spliceNum = 2;
      } else if (this.difficultyLevel === "medium") {
        spliceNum = 4;
      } else if (this.difficultyLevel === "hard") {
        spliceNum = 6;
      }
      const distractions = shuffle([
        ...books.filter(book => book.book !== this.correctBook)
      ]).splice(0, spliceNum);
      this.books = shuffle([
        this.correctBook,
        ...distractions.map(book => book.book)
      ]);
      this.questionText = randVerse.text;
    },
    resetGame: function() {
      if (this.highScore < this.correctQuestions) {
        this.highScore = this.correctQuestions;
      }
      this.currQuestion = 0;
      this.numQuestions = 10;
      this.correctQuestions = 0;
      this.state = "begin";
      this.gameIsRunning = false;
      this.questionText = "";
      this.books = [];
      this.correctBook = "";
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
html,
body,
section,
.hero-body,
#app {
  height: 100%;
  overflow-y: auto;
}
.container {
  padding-bottom: 60px;
}
</style>
