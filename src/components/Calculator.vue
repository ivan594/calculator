<template>
  <div class="mainWrapper">
    <h1>{{ calcTitle }}</h1>

    <div class="calculator">
      <input v-model.number="inputValue" class="input" type="number">
      <div class="buttons">
        <button class="button" @click="onOperationClick('plus')">+</button>
        <button class="button" @click="onOperationClick('minus')">-</button>
        <button class="button" @click="onOperationClick('multiply')">*</button>
        <button class="button" @click="onOperationClick('divide')">/</button>
      </div>
      <button class="button" @click="result">=</button>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'Calculator',
  props: {
    calcTitle: {
      type: String,
      default: 'Май калькулейтар',
    },
  },
  data() {
    return {
      inputValue: 0,
      lastInputValue: 0,
      lastUsedMethod: '',
    };
  },
  computed: {
    operationsMap(): any {
      return {
        plus: this.opPlus,
        minus: this.opMinus,
        multiply: this.opMultiply,
        divide: this.opDivide,
      };
    },
  },
  methods: {
    onOperationClick(operation) {
      this.lastInputValue = this.inputValue;
      this.inputValue = 0;
      this.lastUsedMethod = operation;
    },
    result() {
      const a = this.lastInputValue;
      const b = this.inputValue;
      this.inputValue = this.operationsMap[this.lastUsedMethod](a, b);
    },
    opPlus(a: number, b: number) {
      return a + b;
    },
    opMinus(a: number, b: number) {
      return a - b;
    },
    opMultiply(a: number, b: number) {
      return a * b;
    },
    opDivide(a: number, b: number) {
      return a / b;
    },
  },
  // filters: {
  //   cropString(value: string) {
  //     return value.length > 5 ? `${value.substring(0, 5)}...` : value;
  //   },
  // },
});
</script>

<style scoped>
.mainWrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.calculator {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  max-width: 500px;
  height: 200px;
}
.input {
  height: 40px;
  font-size: 23px;
}
.buttons {
  display: flex;
  justify-content: space-between;
}
.button {
  height: 40px;
  min-width: 40px;
}
</style>
