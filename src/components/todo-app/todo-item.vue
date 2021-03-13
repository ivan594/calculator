<template>
  <div :class="todoClasses">
    <div v-if="!isEditModeEnabled" class="todo__viewMode">
      <div class="todo__text">
        {{ text }}
      </div>
      <div class="todo__deadline">{{ deadline }}</div>
    </div>
    <div v-else class="todo__editMode">
      <input v-model="textInputValue" type="text" class="todo__editModeInput">
      <button class="todo__editModeButton" @click="onSave">Сохранить</button>
    </div>
    <div class="todo__buttons">
      <button class="todo__buttonEdit" @click="toggleEditMode">Edit</button>
      <button class="todo__buttonDelete" @click="onDelete">Delete</button>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';

export default Vue.extend({
  name: 'TodoAppItem',
  props: {
    item: {
      type: Object,
      default: () => ({}),
    },
  },
  data() {
    return {
      isEditModeEnabled: false,
      textInputValue: '',
    };
  },
  computed: {
    priority() {
      return this.item.priority;
    },
    text() {
      return this.item.text || 'Я туду без текста :(';
    },
    deadline() {
      return this.item.deadline;
    },
    createDate() {
      return this.item.createDate;
    },
    id() {
      return this.item.id;
    },
    todoClasses() {
      const classMap = {
        1: 'todo--high',
        2: 'todo--normal',
        3: 'todo--low',
      };
      const priorityToMap = this.priority ? this.priority : 2;
      return [
        'todo',
        classMap[priorityToMap],
        {
          'todo--deadline': true,
        },
      ];
    },
  },
  methods: {
    toggleEditMode() {
      this.isEditModeEnabled = !this.isEditModeEnabled;
    },
    onSave($event) {
      const { id } = this;
      const value = this.textInputValue;
      this.$emit('save', {
        id,
        value,
      }, $event);
      this.isEditModeEnabled = false;
    },
    onDelete($event) {
      const { id } = this;
      this.$emit('delete', {
        id,
      }, $event);
      // this.$emit('delete', {}, $event);
      // console.log('delete me');
    },
  },
});
</script>

<style scoped>
  .todo {
    display: flex;
    justify-content: space-between;
    align-items: center;
    box-sizing: border-box;
    padding: 8px;
    margin-bottom: 16px;
  }
  .todo.todo--high {
    background-color: #B0BF3F;
  }
  .todo.todo--normal {
    background-color: #838C3B;
  }
  .todo.todo--low {
    background-color: #BFBBB4;
  }
  .todo__text {
    max-width: 450px;
  }
  .todo__deadline {
    margin-top: 8px;
  }
  .todo__buttons {
    min-width: 100px;
  }
</style>
