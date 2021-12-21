<template>
  <div class="calc">
    {{ message }}
    <input
      type="number"
      placeholder="operand1"
      id="input_operand1"
      v-model.lazy.number="operand1"
      @click="activeOperand = 'operand1'"
    />
    <input
      type="number"
      placeholder="operand2"
      id="input_operand2"
      v-model.lazy.number="operand2"
      @click="activeOperand = 'operand2'"
    />

    <br />
    <div>
      <div v-if="num_buttons_show === false">
        <button @click="num_buttons_show = true">
          Показать экранную клавиатуру
        </button>
      </div>
      <div v-if="num_buttons_show === true">
        <button @click="num_buttons_show = false">
          Скрыть экранную клавиатуру
        </button>
      </div>
    </div>
    <div class="num_buttons" v-show="num_buttons_show">
      <div class="radiobutton">
        <input
          type="radio"
          id="radio-operand1"
          value="Первое число"
          v-model="picked"
          @change="chooseOperand(picked)"
        />
        <label for="radio-operand1">Первое число</label>
        <br />
        <input
          type="radio"
          id="radio-operand2"
          value="Второе число"
          v-model="picked"
          @change="chooseOperand(picked)"
        />
        <label for="radio-operand2">Второе число</label>
        <br />
      </div>
      <button
        v-for="(number, index) in numbers"
        :key="index"
        @click="inputNumbers(number)"
      >
        {{ number }}
      </button>
    </div>
    <br />
    <button
      v-for="(btn, index) in btns"
      :key="index"
      @click="calculate(btn)"
      :disabled="operand1 === '' || operand2 === ''"
    >
      {{ btn }}
    </button>
    <button v-for="(btn, index) in clearBtns" :key="index" @click="clear(btn)">
      {{ btn }}
    </button>

    <br />

    <div v-show="error">
      {{ error }}
    </div>

    <div v-show="error === ''">
      <p>Результат:</p>
      {{ result }}
      <br />
      <!-- {{ resultFib }} -->
    </div>

    <div class="start-message">
      <template v-if="result < 0">Отрицательное число</template>
      <template v-else-if="result <= 100">Результат в первой сотне</template>
      <template v-else>Результат больше 100</template>
    </div>

    <br />
    <div class="logsList">
      <div class="logItem" v-for="(item, id) in logs" :key="id">
        {{ item }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Calculate",
  props: {
    msg: String,
  },
  data() {
    return {
      message: "Введите два числа",
      result: "",
      // resultFib: 0,
      operand1: "",
      operand2: "",
      error: "",
      btns: ["+", "-", "*", "/", "^", "int /"],
      clearBtns: ["<-", "C"],
      numbers: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9],
      activeOperand: "",
      logs: {},
      picked: "",
      num_buttons_show: false,
    };
  },
  methods: {
    chooseOperand(picked) {
      if (picked === "Первое число") {
        this.activeOperand = "operand1";
      } else {
        this.activeOperand = "operand2";
      }
    },

    inputNumbers(number) {
      if (this.activeOperand === "operand1") {
        this.operand1 += number;
      } else {
        this.operand2 += number;
      }
    },

    // fib(n) {
    //   return n <= 1 ? n : this.fib(n - 1) + this.fib(n - 2);
    // },
    calculate(operation = "+") {
      this.error = "";
      this.result = "";

      this.operand1 = parseInt(this.operand1);
      this.operand2 = parseInt(this.operand2);

      switch (operation) {
        case "+":
          this.add();
          break;

        case "-":
          this.substract();
          break;

        case "*":
          this.multiply();
          break;

        case "/":
          this.divide();
          break;

        case "^":
          this.exponention();
          break;

        case "int /":
          this.intDiv();
          break;
      }

      const key = Date.now();
      const value = `${this.operand1}${operation}${this.operand2} = ${this.result}`;
      this.$set(this.logs, key, value);
      // this.logs[ Date.now()] = `${this.operand1}${operation}${this.operand2} = ${this.result}`;
    },

    clear(operation = "") {
      switch (operation) {
        case "<-":
          this.backspace();
          break;
        case "C":
          this.clearAll();
          break;
      }
    },
    add() {
      this.result = this.operand1 + this.operand2;
      // this.resultFib = this.fib1 + this.fib2;
    },
    substract() {
      this.result = this.operand1 - this.operand2;
    },
    multiply() {
      this.result = this.operand1 * this.operand2;
    },
    divide() {
      const { operand2 } = this;
      if (operand2 === 0) {
        this.error = "Делить на 0 нельзя";
        return;
      }
      this.result = this.operand1 / this.operand2;
    },
    exponention() {
      this.result = this.operand1 ** this.operand2;
    },
    intDiv() {
      const { operand2 } = this;
      if (operand2 === 0) {
        this.error = "Делить на 0 нельзя";
        return;
      }
      this.result = parseInt(this.operand1 / this.operand2);
    },
    backspace() {
      if (this.activeOperand === "operand1") {
        let arrayOfDigits = Array.from(String(this.operand1), Number);
        arrayOfDigits.splice(-1, 1);
        this.operand1 = arrayOfDigits.join("");
      } else {
        let arrayOfDigits = Array.from(String(this.operand2), Number);
        arrayOfDigits.splice(-1, 1);
        this.operand2 = arrayOfDigits.join("");
      }
    },
    clearAll() {
      (this.operand1 = ""), (this.operand2 = ""), (this.result = "");
    },
  },
  computed: {
    // fib1() {
    //   return this.fib(this.operand1);
    // },
    // fib2() {
    //   return this.fib(this.operand2);
    // },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
