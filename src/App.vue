<template>
  <div id="app">
    <h1>Let's play a numeric game!</h1>
    <Message :message="message" v-if="isStateAnswer"/>
    <Counter amount="10" :timeout-handler="timeoutHandler" v-if = "isStateRun"/>
    <Question :question="question" v-if="isStateRun"/>
    <AnswerForm v-on:checked="check" v-if="isStateRun"/>
    <button v-on:click="startClicked" v-if="isStateIdle || isStateAnswer">Start game!</button>
  </div>
</template>

<script>
import Question from "./components/Question.vue";
import Counter from "./components/Counter.vue";
import AnswerForm from "./components/AnswerForm.vue";
import Message from "./components/Message.vue";
import {generateQuestion} from "./services/questions.js";
import {checkAnswer} from "./services/questions";

const GAME_STATE_IDLE = "idle";
const GAME_STATE_RUN = "run";
const GAME_STATE_SHOW_ANSWER = "showAnswer";

export default {
  name: "App",
  components: {
    Question,
    Counter,
    AnswerForm,
    Message
  },
  data: () => ({
    gameState: GAME_STATE_IDLE,
    question: null,
    userAnswer: "",
    message: ""
  }),
  computed: {
    isStateRun() {
      return this.$data.gameState === GAME_STATE_RUN;
    },
    isStateAnswer() {
      return this.$data.gameState === GAME_STATE_SHOW_ANSWER;
    },
    isStateIdle() {
      return this.$data.gameState === GAME_STATE_IDLE;
    },
  },
  methods: {
    timeoutHandler() {
      this.$data.message = "Time is over!";
      this.$data.gameState = GAME_STATE_IDLE;
    },
    check(answer) {
      if (checkAnswer(this.$data.question, Number.parseInt(answer))) {
        this.$data.message = "Correct!";
        this.question = generateQuestion();
      } else {
        this.$data.message = "Wrong!";
      }
      this.$data.gameState = GAME_STATE_SHOW_ANSWER;
    },
    startClicked() {
      this.$data.question = generateQuestion();
      this.$data.gameState = GAME_STATE_RUN;
    }
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
