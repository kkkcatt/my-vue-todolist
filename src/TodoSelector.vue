<script setup>
   import {defineEmits, defineProps} from 'vue';
   const props = defineProps({
  currentFilter: {
    type: String,
    default: 'all'
  },
  itemsLeft: {
    type: Number,
    default: 0
  }
});

const emit = defineEmits(['set-filter', 'clear-completed']);

const setFilter = (filter) => {
  emit('set-filter', filter);
};

const clearCompleted = () => {
  emit('clear-completed');
};
</script>

<template>
 <footer>
      <div class="flex items-stretch text-xs mr-6">
        <span class="p-3"><i>{{ itemsLeft }}</i>   items left</span>
        <button @click="setFilter('all')"  class="border-2 border-transparent p-1 hover:text-pink-800 focus:bg-pink-200 hover:border-white rounded-md">All</button>
        <button @click="setFilter('active')" class=" border-2 border-transparent p-2  hover:text-pink-800 focus:bg-pink-200 hover:border-white rounded-md">Active</button>
        <button @click="setFilter('completed')" class=" border-2 border-transparent p-2  hover:text-pink-800 focus:bg-pink-200 hover:border-white rounded-md">Completed</button>
        <button @click="clearCompleted" class=" border-2 border-transparent p-2  hover:text-pink-800 focus:bg-pink-200  hover:border-white rounded-md">Clear Completed</button>
      </div>
    </footer>
</template>


