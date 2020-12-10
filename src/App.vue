<template>
  <div id="app">
    <Header :numCorrect="numCorrect" :numTotal="numTotal" />
    <b-container class="bv-example-row">
      <b-row class="mt-5 d-flex justify-content-center">
        <b-col sm="8">
          <div v-if="index >= 10">
            <div class="alert alert-success" role="alert">
              <p>You completed test!</p>
              <p>You answered correctly {{ numCorrect }} of {{ numTotal }}.</p>
            </div>
            <button @click.prevent="reloadPage" class="btn btn-success mb-5">
              Start again
            </button>
          </div>
          <QuestionBox
            v-if="questions.length && index < 10"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
    };
  },
  methods: {
    next: function () {
      this.index++;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++;
      }
      this.numTotal++;
    },
    reloadPage: () => {
      window.location.reload();
    },
  },
  mounted: function () {
    fetch("https://opentdb.com/api.php?amount=10&category=28&type=multiple", {
      method: "get",
    })
      .then((response) => {
        return response.json();
      })
      .then((jsonData) => {
        this.questions = jsonData.results;
      });
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
