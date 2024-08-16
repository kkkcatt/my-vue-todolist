<script setup>
    import { ref, defineEmits } from 'vue';
    const emit = defineEmits(['add-todo']);
    const newTodo = ref('');
    const handleSubmit = () => {
        if (newTodo.value.trim()) {
            emit ('add-todo',newTodo.value.trim())
            newTodo.value = ''
        }
    }
</script>

<template>
        <input
        v-model="newTodo"
        @keyup.enter="handleSubmit"  type="text" 
        class="font-medium bg-pink-100 flex-1 w-8 p-5 pl-8 text-3xl border-hidden focus:outline-pink-200 focus:bg-white text-stone-800
         placeholder:text-zinc-300" placeholder="Enter Your todos" autofocus>
</template>
