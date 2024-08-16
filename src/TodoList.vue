<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
  todos: {
    type: Array,
    default: () => [] 
  }
});
const emit = defineEmits(['delete-todo', 'toggle-todo']);
const handleLeftClick = (event, todo) => {
  event.preventDefault();
  emit('toggle-todo', todo);
};

const handleRightClick = (event, todo) => {
  event.preventDefault();
  emit('delete-todo', todo);
};
</script>

<template>
     <div>
        <ul role="list" class="flex flex-col">
          <li  
          v-for="todo in todos" 
          :key="todo.id" 
          @click="handleLeftClick($event, todo)"  
          @contextmenu.prevent="handleRightClick($event, todo)"
          class=" border-b border-stone-300 text-2xl pl-8 p-4">
          <span :class="{ 'line-through text-zinc-300': todo.completed }">{{ todo.text }}</span>
          </li>
        </ul>
      </div>
</template>
