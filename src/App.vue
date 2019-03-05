<template>
  <div id="app">
    <MainBanner v-if="state == 'begin'" v-bind:handleStateChange="handleStateChange"/>
    <ScoreScreen v-if="state == 'score'"/>
    <Question
      v-if="state == 'question'"
      v-bind:buttons="['one', 'two', 'three']"
      difficulty="Easy"
      streak="10"
      question="This is a question"
      points="2"
    />
    <Loading v-if="state == 'loading'"/>
    <Footer/>
  </div>
</template>

<script>
import MainBanner from "./components/MainBanner.vue";
import ScoreScreen from "./components/ScoreScreen.vue";
import Question from "./components/Question.vue";
import Loading from "./components/Loading.vue";
import Footer from "./components/Footer.vue";

export default {
  name: "app",
  components: {
    MainBanner,
    ScoreScreen,
    Question,
    Loading,
    Footer
  },
  data() {
    return {
      state: "begin"
    };
  },
  methods: {
    stateHandler: function() {
      return {
        begin: function() {
          this.state = "begin";
        },
        score: function() {
          this.state = "score";
        },
        loading: function() {
          console.log(this.state);
          this.state = "loading";
        },
        question: function() {
          this.state = "question";
        }
      };
    },
    handleStateChange: function(newState) {
      console.log({ newState });
      this.state = newState;
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
</style>
