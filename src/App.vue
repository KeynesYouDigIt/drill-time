<template>
  <div id="app">
    <ul>
      <li v-for="q in Questions" :key="q.id">
        {{ q.question }}
        you've answered -- {{ q.userAnswer }}
        <div v-for="c in q.choices" :key="c.id">
          <input
            type="radio"
            :value="c.text"
            v-bind:checked="c.checked"
            v-model="q.userAnswer"
            :disabled="disableChoice(q, c.id)"
          >
          <label for="c.text">{{ c.text }}</label>
        </div>
      </li>
    </ul>
    <br>
    <input v-model="newquestion" placeholder="Add question here!">
    <br>
    <textarea v-model="newchoices" placeholder="Add multiple choice answers seperated by a comma"></textarea>
    <br>
    <input v-model="newCorrectAnswer" placeholder="Add correct answer">
    <br>
    <button class="button btn-primary" @click="addQuestion">Add Question</button>
    <br>
    <br>
    <button class="button btn-primary" @click="save">Save Progress</button>
  </div>
</template>

<script>
export default {
  name: "app",
  data: () => {
    return {
      newquestion: "",
      newchoices: "",
      newCorrectAnswer: "",
      Questions: [
        {
          id: 1,
          question: "demo - what do if machine broke?",
          choices: [
            { id: 1, text: "give up" },
            { id: 2, text: "cry" },
            { id: 3, text: "fix" }
          ],
          correctAnswer: "fix",
          userAnswer: ""
        }
      ]
    };
  },
  mounted() {
    localStorage.clear();
  },
  methods: {
    addQuestion: function() {
      var latestQuestionId = this.Questions[this.Questions.length - 1].id;
      var newQuestion = {
        id: latestQuestionId + 1,
        question: this.newquestion,
        choices: makeChoices(this.newchoices),
        correctAnswer: this.newcorrectAnswer,
        userAnswer: ""
      };
      this.Questions.push(newQuestion);
    },
    disableChoice: (question, choice) => {
      if (question.userAnswer && question.userAnswer.id != choice.id) {
        return true;
      }
      return false;
    },
    save: function() {
      this.Questions.map(q => {
        localStorage.setItem(
          q.id,
          //json.parse to get me out!
          JSON.stringify({
            id: q.id,
            question: q.question,
            userAnswer: q.userAnswer,
            correct: q.userAnswer == q.correctAnswer
          })
        );
      });
    }
  }
};

var makeChoices = choicesString => {
  var idnum = 0;
  return choicesString.split(",").map(choiceStr => {
    idnum++;
    return { id: idnum, text: choiceStr };
  });
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
