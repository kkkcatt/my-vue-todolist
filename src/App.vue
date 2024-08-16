<script setup >
import { ref, computed,onMounted, watch } from 'vue';
import TodoInput from './TodoInput.vue';
import TodoList from './TodoList.vue';
import TodoSelector from './TodoSelector.vue';

// 从 localStorage 加载待办事项
const loadTodos = () => {
  const savedTodos = localStorage.getItem('todos');
  if (savedTodos) {
    return JSON.parse(savedTodos);
  }
  return [];
};
const todos = ref(loadTodos());
const filter = ref('all'); // 默认过滤器设置为 'all'

// 观察 todos 变化并保存到 localStorage
watch(todos, (newTodos) => {
  localStorage.setItem('todos', JSON.stringify(newTodos));
}, { deep: true });


const addTodo = (todo) => {
  if (todo) {
    todos.value.push({
      id: Date.now(), 
      text: todo,
      completed: false
    });
  }
};
const deleteTodo = (todoToDelete) => {
  todos.value = todos.value.filter(todo => todo.id !== todoToDelete.id);
};

const toggleTodo = (todoToToggle) => {
  const todo = todos.value.find(t => t.id === todoToToggle.id);
  if (todo) {
    todo.completed = !todo.completed;
  }
};
// 计算未完成的待办事项数
const itemsLeft = computed(() => {
  return todos.value.filter(todo => !todo.completed).length;
});
// 根据当前过滤器过滤待办事项
const filteredTodos = computed(() => {
  if (filter.value === 'all') return todos.value;
  if (filter.value === 'active') return todos.value.filter(todo => !todo.completed);
  if (filter.value === 'completed') return todos.value.filter(todo => todo.completed);
});

const clearCompleted = () => {
  todos.value = todos.value.filter(todo => !todo.completed);
};


// 处理过滤器设置
const setFilter = (newFilter) => {
  filter.value = newFilter;
};

// 在组件加载时检查 localStorage 中的过滤器设置
onMounted(() => {
  const savedFilter = localStorage.getItem('filter');
  if (savedFilter) {
    filter.value = savedFilter;
  }
});

// 观察 filter 变化并保存到 localStorage
watch(filter, (newFilter) => {
  localStorage.setItem('filter', newFilter);
});
</script>

<template>
    <div class="min-h-screen flex flex-col items-center justify-center">
    <div class=" flex justify-center">
      <h1 class="text-8xl text-pink-200 pb-44 font-bold" >小Zの记事本</h1>
    </div>

    <main class="bg-pink-100 shadow-2xl w-9/12 max-w-lg">
      <div class=" flex rounded-md border-b border-stone-300">
        <TodoInput @add-todo='addTodo'/>
      </div>
      <TodoList 
        :todos="filteredTodos"
        @delete-todo="deleteTodo"  
        @toggle-todo="toggleTodo"
      />
      <TodoSelector 
        :itemsLeft="itemsLeft"
        :currentFilter="filter" 
        @set-filter="setFilter" 
        @clear-completed="clearCompleted" 
      />
    </main>

    <small class="p-14 text-l text-center text-stone-400">Left Click to toggle Completed.<br>
          Right Click to delete todo
    </small>
  </div>
</template>
