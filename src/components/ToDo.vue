<script setup>
defineProps(['todos'])
</script>
<template>

    <ul>
        <li v-for="(todo,index) in todos" :key="todo.title">
        {{todo.title}} 
        <button @click="$emit('toggleComplete',index)">{{todo.completed?"UnComplete":"Complete"}}</button>
        <button @click="$emit('deleteTodo',index)">delete</button>
        </li>
    </ul>
</template>