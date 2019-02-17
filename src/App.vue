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
    <input v-model="newquestion" placeholder="Add question here!">
    <textarea v-model="newchoices" placeholder="Add multiple choice answers seperated by a comma"></textarea>
    <input v-model="newCorrectAnswer" placeholder="Add correct answer">
    
    <button class="button btn-primary" @click="addQuestion">Add Question</button>
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
  methods: {
    addQuestion: function() {
      var newQuestion = {
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
