<template>
  <div class="mainWrapper">
    <h1>{{ calcTitle }}</h1>

    <div class="calculator">
      <input v-model.number="inputValue" class="input" type="number">
      <div class="buttons">
        <button class="button" @click="onOperationClick(EOperations.PLUS)">+</button>
        <button class="button" @click="onOperationClick(EOperations.MINUS)">-</button>
        <button class="button" @click="onOperationClick(EOperations.MULTIPLY)">*</button>
        <button class="button" @click="onOperationClick(EOperations.DIVIDE)">/</button>
      </div>
      <button class="button" @click="result">=</button>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
enum EOperations {
  PLUS = 'opPlus',
  MINUS = 'opMinus',
  MULTIPLY = 'opMultiply',
  DIVIDE = 'opDivide'
}
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
      EOperations,
    };
  },
  methods: {
    onOperationClick(operation: string) {
      if (this.lastUsedMethod) {
        const a = this.lastInputValue;
        const b = this.inputValue;
        this.lastInputValue = this[this.lastUsedMethod](a, b);
        this.inputValue = 0;
        this.lastUsedMethod = operation;
        return;
      }
      this.lastInputValue = this.inputValue;
      this.inputValue = 0;
      this.lastUsedMethod = operation;
    },
    result() {
      if (!this.lastUsedMethod) {
        return;
      }
      const a = this.lastInputValue;
      const b = this.inputValue;
      this.inputValue = this[this.lastUsedMethod](a, b);
      this.lastUsedMethod = undefined;
    },
    [EOperations.PLUS](a: number, b: number) {
      return a + b;
    },
    [EOperations.MINUS](a: number, b: number) {
      return a - b;
    },
    [EOperations.MULTIPLY](a: number, b: number) {
      return a * b;
    },
    [EOperations.DIVIDE](a: number, b: number) {
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
