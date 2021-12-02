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
    <button v-on:click="sum()">+</button>
    <button v-on:click="minus()">-</button>
    <button v-on:click="mul()">*</button>
    <button v-on:click="div()" :disabled="operand2 == 0">/</button>
    <br />
    <button v-on:click="stepen()">Степень</button>
    <button v-on:click="intDiv()">Целочисленное деление</button>

    <p>Результат:</p>
    {{ result }}
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
    };
  },
  methods: {
    sum() {
      const { operand1, operand2 } = this;
      this.result = operand1 + operand2;
    },
    minus() {
      this.result = this.operand1 - this.operand2;
    },
    mul() {
      this.result = this.operand1 * this.operand2;
    },
    div() {
      this.result = this.operand1 / this.operand2;
    },
    stepen() {
      this.result = this.operand1 ** this.operand2;
    },
    intDiv() {
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
