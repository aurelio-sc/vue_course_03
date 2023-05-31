<template>
  <div class="wrapper">
    <template v-if="this.question">
      <h1 class="question" v-html="this.question"></h1> 
      <div class="options">
        <template v-for="(answer, index) in this.answers" :key="index">
          <div class="option">
            <input
              type="radio"
              :value="answer"
              :name="answer"
              :id="answer" >
            <label
              :for="answer"
              v-html="answer">
            </label>
          </div>
        </template>
      </div> 
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
      correctAnswer: undefined
    }
  },
  computed: {
    answers() {
      var answers = JSON.parse(JSON.stringify(this.incorrectAnswers));
      answers.splice(Math.round(Math.random() * answers.length), 0, this.correctAnswer);
      return answers;
    }
  },
  created() {
    this.axios
    .get('https://opentdb.com/api.php?amount=1&category=18')
    .then((response) => {
      this.question = response.data.results[0].question;
      this.incorrectAnswers = response.data.results[0].incorrect_answers;
      this.correctAnswer = response.data.results[0].correct_answer;

    })
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
    border: 1px solid #2c3e50;
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
      margin-top: 12px;
      height: 40px;
      min-width: 120px;
      padding: 0 16px;
      color: #FFF;
      background-color: #1867C0;
      border: 1px solid #1867C0;
      cursor: pointer;
    }
  }
}
</style>
