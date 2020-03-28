<template>
  <div id="test">
    <h2>Math Quiz</h2>

    <b-container>
      <b-row v-for="equation in equations" :key="equation.id" class="justify-content-md-center">
        <b-col
          class="my-b-col"
          v-for="(el, i) in equation.elements"
          :key="`${el}_${i}`"
          col
          sm="1"
        >{{el}}</b-col>
        <b-col class="my-b-col-answer" col sm="1">
          <b-form-input class="my-b-form-input" v-model="answers[equation.id]" placeholder="..."></b-form-input>
        </b-col>
        <b-col v-if="results[equation.id] === 'right'" class="right my-b-col" col sm="1">GOOD</b-col>
        <b-col v-if="results[equation.id] === 'wrong'" class="wrong my-b-col" col sm="1">BAD</b-col>
      </b-row>

      <b-row class="justify-content-md-center">
        <b-col col sm="2">
          <b-button variant="primary" @click="submitAnswers">Submit</b-button>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
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
      return (this.results = temp_results);
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
.my-b-col {
  font-size: 25px;
  background-color: lightskyblue;
  margin: 0px 5px;
  padding: 5px 0 0 0;
  border-radius: 5px;
  text-align: center;
}

.wrong {
  background-color: lightcoral;
}

.right {
  background-color: lightgreen;
}

.justify-content-md-center {
  margin: 40px 0px;
}

.my-b-col-answer {
  font-size: 25px;
  background-color: lightgrey;
  margin: 0px 5px;
  border-radius: 5px;
  text-align: center;
}

.my-b-form-input {
  text-align: center;
  font-size: 25px;
  border: lightgrey;
  background-color: lightgrey;
}
</style>