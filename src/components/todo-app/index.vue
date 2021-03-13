<template>
  <div class="mainWrapper">
    <h1 class="mainTitle">Мой список дел :)</h1>

    <div class="todoControls">
      <select
        v-model.number="prioritySelectValue"
        class="todoControls__prioritySelect"
      >
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
      </select>
      <input
        v-model="textInputValue"
        type="text"
        placeholder="Введите текст"
        class="todoControls__textInput"
      >
      <input
        v-model="deadlineInputValue"
        type="text"
        placeholder="Введите deadline"
        class="todoControls__dateInput"
      >
      <button class="todoControls__addButton" @click="addNewItem">+</button>
    </div>

    <div class="todoList">
      <TodoAppItem
        v-for="todoItem of innerTasks"
        :key="todoItem.id"
        :item="todoItem"

        @save="onTodoAppItemSave"
        @delete="deleteTodo"
      />
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import TodoAppItem from './todo-item.vue';
import TodoItem from './todo-item-class';

export default Vue.extend({
  name: 'TodoApp',
  components: {
    TodoAppItem,
  },
  props: {
    tasks: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      innerTasks: [],
      prioritySelectValue: 1,
      textInputValue: '',
      deadlineInputValue: '',
    };
  },
  created() {
    this.innerTasks = this.tasks;
  },
  methods: {
    addNewItem() {
      const newItem = new TodoItem({
        priority: this.prioritySelectValue,
        text: this.textInputValue,
        deadline: this.deadlineInputValue,
      });
      this.innerTasks.unshift(newItem);
    },
    onTodoAppItemSave(payload, $event): void {
      const { id, value } = payload;
      const newId = id;
      const newValue = value;
      const innerTasksValue = this.tasks;
      const newObject = innerTasksValue.find((obj) => obj.id === newId);
      newObject.text = newValue;
    },
    deleteTodo(payload, $event): void {
      const { id } = payload;
      const innerTasksValueDelete = this.tasks;
      const index = innerTasksValueDelete.findIndex((obj) => obj.id === id);
      innerTasksValueDelete.splice(index, 1);
      console.log('Я пытаюсь удалить этот объект');
    },
  },
});
</script>

<style scoped>
  .mainWrapper {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    min-height: 500px;
  }
  .todoControls {
    margin: 16px 0;
  }
  .todoList {
    width: 100%;
    max-width: 800px;
    padding: 16px;
    box-sizing: border-box;
  }
</style>
