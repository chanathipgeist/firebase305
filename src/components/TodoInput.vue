<script setup>
import {ref} from 'vue'
import {collection,addDoc, serverTimestamp} from 'firebase/firestore'
import db from '../firebase/init.js'

const emit = defineEmits(['update-todos'])
const newTitle = ref('')

async function addTodo(){
     await addDoc(collection(db,"todos"),{
        createdAt: serverTimestamp(),
        completed: false,
        title: newTitle.value
     })
     emit('update-todos')
    console.log(newTitle.value)
}
</script>
<template>
<div>
    <input v-model="newTitle" placeholder ="Enter a new Task." size="30" />
    <button @click="addTodo" >ADD</button>
</div>
    
</template>