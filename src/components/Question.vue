<template>
  <div id="questionBody">
    <div class="row">
      <div class="row">
        <p>Difficulty:</p>
        <p>{{ difficulty }}</p>
      </div>
      <div class="row">
        <p>Time left:</p>
        <p>{{ countdown }}</p>
      </div>
      <div class="row">
        <p>Score:</p>
        <p>{{ score }}</p>
      </div>
    </div>
    <div class="col">
      <p>{{ selectedQuestion.question }}</p>
      <div class="row">
        <span v-for="(a, i) in answers" v-bind:key="i" class="answer">
          <input
            type="radio"
            :value="a"
            v-model="selectedAnswer"
            :checked="isChecked"
            @change="checkAnswer()"
            :disabled="isChecked"
          />{{ a }}
        </span>
      </div>
    </div>
    <div v-if="isChecked">
      <div v-if="!isCorrect">
        <p>Wrong! the right answer is: {{ selectedQuestion.correct }}</p>
      </div>
      <div v-else>
        <p>Correct!</p>
      </div>
    </div>
  </div>
</template>

<script>
import triviaQuestions from "../data/triviaQuestions.json";

export default {
  props: {
    difficulty: {
      type: String,
    },
  },
  data() {
    return {
      answers: [],
      index: 0,
      isChecked: false,
      triviaQuestions,
      selectedAnswer: "",
      selectedQuestion: "",
      countdown: 0,
      score: 0,
      isCorrect: undefined,
      difficultyManuel: {
        easy: {
          timer: 30,
          correct: 15,
          incorrect: -5,
        },
        medium: {
          timer: 15,
          correct: 10,
          incorrect: -10,
        },
        hard: {
          timer: 5,
          correct: 5,
          incorrect: -15,
        },
      },
      difficultySelected: "",
    };
  },
  methods: {
    //SHFFELING ALGORITHM
    shuffle(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array;
    },
    //Checking if the selected answer is correct
    checkAnswer() {
      this.isChecked = true;
      if (this.selectedAnswer === this.triviaQuestions[this.index].correct) {
        this.score += this.difficultyManuel[this.difficulty].correct;
        this.isCorrect = true;
      } else {
        this.score += this.difficultyManuel[this.difficulty].incorrect;
        this.isCorrect = false;
      }
    },
    //Chenging the question that is being presented
    changeQuestion() {
      if (this.selectedAnswer === "")
        this.score += this.difficultyManuel[this.difficulty].incorrect;
      this.selectedQuestion = this.triviaQuestions[this.index];
      this.selectedAnswer = "";
      this.isChecked = false;
      this.isCorrect = undefined;
    },
    //Adding the inccorect answer and shuffeling the answers
    shuffleAnswers(question) {
      var answers = [...question.incorrect];
      answers.push(question.correct);
      this.answers = this.shuffle(answers);
    },
  },
  //Watching over the countdown
  watch: {
    countdown: {
      handler(value) {
        if (value > 0) {
          setTimeout(() => {
            this.countdown--;
          }, 1000);
        } else {
          this.countdown = this.difficultyManuel[this.difficulty].timer; //resetting the countdown
        }
      },
      immediate: true, // This ensures the watcher is triggered upon creation
    },
  },
  //Mounting the initial question and answers
  beforeMount() {
    if (this.index === 0) {
      this.selectedQuestion = this.triviaQuestions[0];
      this.shuffleAnswers(this.triviaQuestions[0]);
    }
  },
  //Mounting the new question and answers every X seconds
  mounted() {
    var inter = window.setInterval(() => {
      if (this.index + 1 < this.triviaQuestions.length) {
        this.index++;
        this.shuffleAnswers(this.triviaQuestions[this.index]);
        this.changeQuestion();
      }

      //When we reach the end of the trivia array we stop the interval and emit true to the parent component
      else {
        clearInterval(inter);
        this.$emit("triviaDone", true);
        this.$emit("endScore", this.score);
      }
    }, this.countdown * 1000);
  },
};
</script>

<style scoped>
.row {
  justify-content: center;
  flex-wrap: nowrap;
  justify-content: space-around;
}

.answer {
  padding: 0 20px;
}

#questionBody {
  margin-top: 15em;
}
</style>
