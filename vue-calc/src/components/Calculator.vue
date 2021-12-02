<template>
  <div class="calc">
    {{ message }}
    <input
      type="number"
      placeholder="operand1"
      v-model.lazy.number="operand1"
    />
    <input
      type="number"
      placeholder="operand2"
      v-model.lazy.number="operand2"
    />
    <br />
    <button
      v-for="(btn, index) in btns"
      :key="index"
      @click="calculate(btn)"
      :disabled="operand1 === '' || operand2 === ''"
    >
      {{ btn }}
    </button>

    <br />

    <div v-show="error">
      {{ error }}
    </div>

    <div v-show="error === ''">
      <p>Результат:</p>
      {{ result }}
    </div>

    <div class="start-message">
      <template v-if="result < 0">Отрицательное число</template>
      <template v-else-if="result <= 100">Результат в первой сотне</template>
      <template v-else>Результат больше 100</template>
    </div>
    <div class="list">
      <div v-for="(item, index) in myColletion" :key="index">
        {{ item }} - {{ index }}
      </div>
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
      result: 0,
      operand1: "",
      operand2: "",
      error: "",
      btns: ["+", "-", "*", "/", "^", "int /"],
      myColletion: [1, 2, 3, 4, 5, 6, 7, 8, 9],
      logs: {},
    };
  },
  methods: {
    calculate(operation = "+") {
      this.error = "";
      this.result = "";
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
    add() {
      this.result = this.operand1 + this.operand2;
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
