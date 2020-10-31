<template>
  <div id="app">
    <Landing
      v-if="!name"
      :difficulty="difficulty"
      @inputData="inputName"
      @selectedDifficult="selectedDifficulty"
    />
    <div v-else-if="!finished">
      <question
        :difficulty="difficultySelected"
        @triviaDone="finish"
        @endScore="triviaScore"
      />
    </div>
    <div v-else class="col" id="result">
      <p>Hey, {{ name }}, good try!</p>
      <p>Your final score is: {{ score }}</p>
    </div>
  </div>
</template>

<script>
import Question from "./components/Question.vue";
import Landing from "./components/Landing.vue";

export default {
  name: "App",
  components: {
    Question,
    Landing,
  },
  data() {
    return {
      name: "",
      difficulty: ["easy", "medium", "hard"],
      difficultySelected: "",
      finished: false,
      score: 0,
    };
  },
  methods: {
    inputName(name) {
      this.name = name;
    },
    selectedDifficulty(dif) {
      this.difficultySelected = dif;
    },
    finish(isFinished) {
      this.finished = isFinished;
    },
    triviaScore(score) {
      this.score = score;
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Inconsolata:wght@300;600;900&display=swap");
* {
  font-family: "Inconsolata", monospace;
}

.col {
  display: flex;
  display: -webkit-flex;
  flex-flow: wrap;
  flex-direction: column;
  -webkit-flex-flow: wrap;
  align-items: center;
  -webkit-align-items: center;
}

.row {
  display: flex;
  display: -webkit-flex;
  flex-flow: wrap;
  flex-direction: row;
  -webkit-flex-flow: wrap;
  align-items: center;
  -webkit-align-items: center;
  margin: 10px 0;
}

#result {
  border: 1px solid black;
  padding-top: 15em;
}
</style>
