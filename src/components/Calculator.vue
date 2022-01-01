<template>
  <div>
    <v-container>
      <v-row>
        <v-col></v-col>
        <v-col><h1>
          Calculator
        </h1></v-col>
        <v-col></v-col>
      </v-row>
      <v-row align="center"
             justify="center">
        <v-col></v-col>
        <v-col align-self="center"
               justify="center"
        >
          <v-card
              class="text-center">
            <v-text-field
                v-model="input"
                outlined
                solo
                label="type equation here"
            ></v-text-field>
          </v-card>
        </v-col>
        <v-col></v-col>
      </v-row>
      <v-row>
        <v-col cols="4"></v-col>
        <v-col cols="2">
          <v-btn @click="processButton(button)" class="text-lowercase" v-for="button in buttonL" v-bind:key="button.id">
            {{ button }}
          </v-btn>
        </v-col>
        <v-col cols="2">
          <v-btn @click="processButton(button)" class="text-lowercase" v-for="button in buttonR" v-bind:key="button.id">
            {{ button }}
          </v-btn>
        </v-col>
        <v-col cols="4"></v-col>
      </v-row>
    </v-container>
    <h1>{{ compute(input) }}</h1>
  </div>
</template>

<script>
import {evaluate} from "mathjs";

export default {
  name: 'Calculator',
  data() {
    return {
      alignments: [
        'start',
        'center',
        'end',
      ],
      input: "0",
      buttonL: buttonsLeft,
      buttonR: buttonsRight,
      oldVar: "",
      output: "",
    }
  },
  methods: {
    checkIfUpdate: function () {
      this.oldVar = this.input;
      if (this.oldVar !== this.input) {
        return true;
      }
    },
    processButton: function (b) {
      if (this.input === "") {
        this.input = b;
      } else {
        switch (b) {
          case 'sqrt':
            this.input = "sqrt("+ this.input + ")";
            break;
          case '|n|':
            this.input = "|" + this.input + "|";
            break;
          case 'ln':
            this.input = this.input + "ln(";
            break;
          case 'log':
            this.input = this.input + "log(";
            break;
          case '^n':
            this.input = this.input + "^";
            break;
          case 'pi':
            this.input = this.input + "pi";
            break;
          case '!':
            this.input += '!';
            break;
          case 'e':
            this.input += 'e';
            break;
          case '=':
            break;
          default:
            this.input += b;
        }
      }

    },
    checkWorks: function (formula, symbol) {
      switch (symbol) {
        case '.':
          if (formula.match(/\.[\d]*\./)) {
            return false;
          }
          break;
        case '+':
        case '/':
        case '*':
          if (formula.charAt(0) === symbol) {
            return false;
          }
          if (formula.match(`[\\+\\-\\/\\*][\\` + symbol + `]`)) {
            return false;
          }
          break;
        case '-':
          if (formula.match(/--/)) {
            return false;
          }
          break;
      }
      return true;
    },
    compute: function (formula) {
      if (this.checkWorks(formula, '+') && this.checkWorks(formula, '*') && this.checkWorks(formula, '/') && this.checkWorks(formula, '-') && (formula.split(')')).length === (formula.split('(')).length) {

        //formula = formula.replace(/\d\(/, /\d/ + '*(')
        //formula = formula.replace(')(', ')*(')
        //formula = formula.replace('pi', '3.14159265358')
        //formula = formula.replace('e', '2.71828128')
        return evaluate(formula);
      } else if (this.input === "") {
        return 0;
      } else {
        return "doesn't work";
      }
    }


  },
  props: {}

}
let buttonsLeft = ['^2', '^n', 'sqrt', '|n|', 'e', 'pi', 'log', 'ln', '(', ')', '!',];
let buttonsRight = ['7', '8', '9', '/', '4', '5', '6', '*', '3', '2', '1', '-', '0', '.', '=', '+'];
</script>

<style>

h1 {
  text-align: center
}

button {
  width: 65px;
}

.container {
  max-width: 1200px;
}

.calculatorShell {
  max-width: 500px
}
</style>