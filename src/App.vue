<template>
  <div id="app" ref="app">
    <Calculator
      calcTitle="My beautiful calculator!"
    />
    <TodoApp
      :tasks="todoListTasks"
    />
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import Calculator from './components/Calculator.vue';
import TodoApp from './components/todo-app/index.vue';
import TodoItem from './components/todo-app/todo-item-class';

const todoListTasks = [
  new TodoItem({
    priority: 2,
    text: 'Купить еды черепахе',
    deadline: '28.01.2021',
  }),
  new TodoItem({
    priority: 1,
    text: 'Купить еды коту',
    deadline: '26.01.2021',
  }),
  new TodoItem({
    priority: 3,
    text: 'Lorem ipsum dolor sit amet consectetur adipisicing elit. Modi, voluptates.',
    deadline: '30.01.2021',
  }),
];
export default Vue.extend({
  name: 'App',
  components: {
    Calculator,
    TodoApp,
  },
  data() {
    return {
      todoListTasks,
    };
  },
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
