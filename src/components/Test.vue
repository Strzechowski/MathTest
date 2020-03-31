<template>
  <div id="test">
    <h1>Math Quiz</h1>

    <b-container>
      <b-row v-for="equation in equations" :key="equation.id" class="justify-content-center">
        <b-col class="my-b-col" cols="4" >
          {{equation.elements[0]}} {{equation.elements[1]}} {{equation.elements[2]}} {{equation.elements[3]}}
        </b-col>
        <b-col class="my-b-col-answer" cols="3" >
          <b-form-input class="my-b-form-input" :disabled=areAnswersSubmited v-model="answers[equation.id]" placeholder="..."></b-form-input>
        </b-col>
        <b-col v-if="results[equation.id] === 'right'" class="right my-b-col" cols="3">GOOD</b-col>
        <b-col v-if="results[equation.id] === 'wrong'" class="wrong my-b-col" cols="3">BAD</b-col>
      </b-row>

      <b-row class="justify-content-center">
        <b-col>
          <b-button size="lg" v-if=!areAnswersSubmited variant="primary" @click="submitAnswers">Submit</b-button>
          <b-button szie="lg" v-else variant="primary" @click="restartQuiz">Try again!</b-button>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
      areAnswersSubmited: false,
      equations: [],
      answers: [],
      results: []
    };
  },

  mounted() {
    this.createEquations();
  },

  methods: {
    createEquations() {
      var temp_equations = [];
      for (let i = 0; i < 10; i++) {
        var available_operators = ['+', '-', '*', '/'];
        var operator = available_operators[Math.floor(Math.random() * 4)]
        var num1 = Math.floor(Math.random() * 20);
        var num2 = Math.floor(Math.random() * 20);
        // for division we use special rules
        if (operator == '/') {
          num2 = Math.floor(Math.random() * 19) + 1;
          num1 = num2 * Math.floor(Math.random() * 20);
        }
        temp_equations[i] = {
          id: i + 1,
          elements: [num1, operator, num2, "="]
        }
      }
      this.equations = temp_equations
    },
    submitAnswers() {
      let temp_results = [];
      for (let i = 1; i < this.equations.length + 1; i++)
        temp_results[i] = this.checkAnswer(i);
      this.areAnswersSubmited = true
      return (this.results = temp_results);
    },
    restartQuiz() {
      this.results = []
      this.createEquations()
      this.areAnswersSubmited = false
      this.answers = []
    },
    checkAnswer(resultNumber) {
      var equation = this.equations[resultNumber - 1];
      var result = null;
      switch (equation.elements[1]) {
        case "+":
          result =
            equation.elements[0] + (equation.elements[2]);
          break;
        case "-":
          result =
            parseInt(equation.elements[0]) - parseInt(equation.elements[2]);
          break;
        case "*":
          result =
            parseInt(equation.elements[0]) * parseInt(equation.elements[2]);
          break;
        case "/":
          result =
            parseInt(equation.elements[0]) / parseInt(equation.elements[2]);
          break;
      }

      // we need +1 because id's starts from 1
      if (this.answers[resultNumber] == result) {
        return "right";
      } else {
        return "wrong";
      }
    }
  }
};
</script>

<style scoped>
h1 {
  font-size: 7vmin;
}

.my-b-col {
  font-size: 6vmin;
  background-color: lightskyblue;
  margin: 0px 5px;
  padding: 5px 0 0 0;
  border-radius: 5px;
  text-align: center;
}

.wrong {
  background-color: lightcoral;
}

.container-sm {
  max-width: max-content;
}

.right {
  background-color: lightgreen;
}

.justify-content-center {
  padding: 0;
  margin: 40px 0px;
}

.my-b-col-answer {
  padding: 0;
  font-size: 5vmin;
  background-color: lightgrey;
  margin: 0px 5px;
  border-radius: 5px;
  text-align: center;
}

.my-b-form-input {
  text-align: center;
  font-size: 5vmin;
  background-color: lightgrey;
}

</style>