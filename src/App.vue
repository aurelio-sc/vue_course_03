<template>
  <div class="wrapper">
    <template v-if="this.question">
      <h1 class="question" v-html="this.question"></h1> 
      <div class="options">
        <template v-for="(answer, index) in this.answers" :key="index">
          <div class="option">
            <input
              :disabled="this.answerSubmited"
              type="radio"
              :value="answer"
              :name="answer"
              :id="answer" 
              v-model="this.chosenAnswer"
              >
            <label
              :for="answer"
              v-html="answer">
            </label>
          </div>
        </template>
      </div>
      <button v-if="!this.answerSubmited" @click="this.submitAnswer" class="send" type="button">Send</button> 
      <section v-if="this.answerSubmited" class="result">
        <h4 
          v-if="this.chosenAnswer == this.correctAnswer"
          v-html="'&#9989; Congratulations! The answer ' +  this.chosenAnswer + 'is correct!'">
        </h4>
        <h4 v-else
          v-html="'&#10060; I\'m sorry, you picked the wrong answer. The correct one is ' + this.correctAnswer + '.'">
        </h4>
        <button @click="this.getNewQuestion()" class="send" type="button">Next Question</button> 
      </section>
    </template>
  </div>  
</template>

<script>


export default {
  name: 'App',
  
  data() {
    return {
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      chosenAnswer: undefined,
      answerSubmited: false
    }
  },
  computed: {
    answers() {
      var answers = JSON.parse(JSON.stringify(this.incorrectAnswers));
      answers.splice(Math.round(Math.random() * answers.length), 0, this.correctAnswer);
      return answers;
    }
  },
  methods: {
    submitAnswer() {
      if (!this.chosenAnswer) {
        alert('Please pick an option.');
      } else {
        this.answerSubmited = true;
        if (this.chosenAnswer == this.correctAnswer) {
          console.log('Congrats!');
        } else {
          console.log('Better luck next time');
        }
      }
    },
    getNewQuestion() {
      this.answerSubmited = false;
      this.chosenAnswer = undefined;
      this.question = undefined;

      this.axios
      .get('https://opentdb.com/api.php?amount=1&category=18')
      .then((response) => {
        this.question = response.data.results[0].question;
        this.incorrectAnswers = response.data.results[0].incorrect_answers;
        this.correctAnswer = response.data.results[0].correct_answer;

      })
    }
  },
  created() {
    this.getNewQuestion();
  }
}


</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;  
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;   
  .wrapper{
    //border: 1px solid #2c3e50;
    border-radius: 10px;
    padding: 30px;
    display: flex;
    flex-direction: column;
    gap: 30px;
    .question{
      margin: 0;
      text-align: center;
    }
    .options{
      display: flex;
      flex-direction: column;
      gap: 15px;
      width: fit-content;
      margin: 0 auto;
      .option{
        display: flex;
        align-items: center;
        gap: 10px;
        input[type=radio] {
          margin: 0;
        }
      }
    }
    .send{
     display: flex;
     align-items: center;
     justify-content: center;
     width: fit-content;
     padding: 15px 50px;
     text-transform: uppercase;
     font-size: 16px;
     font-weight: 600;
     letter-spacing: 1px;
     border-radius: 10px;
     margin: 0 auto;
      color: #FFF;
      background-color: #1867C0;
      border: 1px solid #1867C0;
      cursor: pointer;
      transition: all 0.2s;
      &:hover{
        background-color: #124a8a;
        border-color: #124a8a;
      }
    }
    .result{
      h4{
        text-align: center;
      }
    }
  }
}
</style>
