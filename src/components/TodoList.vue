<template>
    <div class="todo-list">
      <input 
        v-model="newTodo" 
        @keyup.enter="addTodo"
        placeholder="添加新的待办事项"
      />
      <button @click="addTodo">添加</button>
  
      <ul>
        <li v-for="todo in todos" :key="todo.id">
          <input 
            type="checkbox" 
            :checked="todo.completed" 
            @change="toggleTodo(todo)"
          />
          <span :class="{ completed: todo.completed }">{{ todo.text }}</span>
          <button @click="removeTodo(todo)">删除</button>
        </li>
      </ul>
  
      <div class="todo-stats">
        总计: {{ totalTodos }} | 已完成: {{ completedTodos }}
      </div>
    </div>
  </template>
  
  <script>
  import { ref, reactive, computed } from 'vue'
  
  export default {
    name: 'TodoList',
    setup() {
      const newTodo = ref('')
      const todos = reactive([])
  
      const addTodo = () => {
        if (newTodo.value.trim()) {
          todos.push({
            id: Date.now(),
            text: newTodo.value,
            completed: false
          })
          newTodo.value = ''
        }
      }
  
      const toggleTodo = (todo) => {
        todo.completed = !todo.completed
      }
  
      const removeTodo = (todoToRemove) => {
        const index = todos.findIndex(todo => todo.id === todoToRemove.id)
        if (index !== -1) {
          todos.splice(index, 1)
        }
      }
  
      const totalTodos = computed(() => todos.length)
      const completedTodos = computed(() => todos.filter(todo => todo.completed).length)
  
      return {
        newTodo,
        todos,
        addTodo,
        toggleTodo,
        removeTodo,
        totalTodos,
        completedTodos
      }
    }
  }
  </script>
  
  <style scoped>
  .todo-list {
    max-width: 400px;
    margin: 0 auto;
  }
  
  ul {
    list-style-type: none;
    padding: 0;
  }
  
  li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 0;
    border-bottom: 1px solid #eee;
  }
  
  .completed {
    text-decoration: line-through;
    color: #999;
  }
  
  .todo-stats {
    margin-top: 20px;
    font-style: italic;
  }
  </style>