<template>
  <div class="col">
    <h1>Welcome to Nadia's Trivia!</h1>
    <img src="@/assets/catgif.gif" />
    <div class="row">
      <input type="text" v-model="name" placeholder="What's your name?" />
      <button @click="submitName" :disabled="!name || !selectedDifficulty">
        Enter
      </button>
    </div>
    <p>Difficulty:</p>
    <div class="row">
      <span
        v-for="(dif, index) in difficulties"
        v-bind:key="index"
        class="m-r-10"
        @mousemove="show(dif)"
        @mouseleave="hide(dif)"
      >
        <input v-model="selectedDifficulty" :value="dif" type="radio" />{{
          dif
        }}
      </span>
    </div>
    <div class="row">
      <div class="hidden" id="easy">
        <p>Time to answer a question: 30s</p>
        <p>Answering right: +15</p>
        <p>Answering wrong: -5</p>
      </div>
      <div class="hidden" id="medium">
        <p>Time to answer a question: 15s</p>
        <p>Answering right: +10</p>
        <p>Answering wrong: -10</p>
      </div>
      <div class="hidden" id="hard">
        <p>Time to answer a question: 5s</p>
        <p>Answering right: +5</p>
        <p>Answering wrong: -15</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    difficulty: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      difficulties: this.difficulty,
      name: "",
      selectedDifficulty: "",
      isActive: false,
    };
  },
  methods: {
    submitName() {
      this.$emit("inputData", this.name);
      this.$emit("selectedDifficult", this.selectedDifficulty);
    },
    show(dif) {
      var box = document.getElementById(dif);
      box.className = "active";
    },
    hide(dif) {
      var box = document.getElementById(dif);
      box.className = "hidden";
    },
  },
};
</script>

<style>


.m-r-10 {
  margin-right: 10px;
}

input[type="text"],
input[type="text"]:focus {
  border: none;
  border-bottom: 2px solid grey;
  padding: 3px;
  margin-right: 5px;
  outline: none;
}

.hidden {
  display: none;
}

.active {
  display: block;
  border: 1px solid black;
  padding: 0 20px;
}
</style>
