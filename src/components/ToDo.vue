<script setup>
import {query, collection, getDocs } from 'firebase/firestore'
import { ref, onMounted } from 'vue';
import db from '../firebase/init.js'

const todos = ref([])

    async function getTodos () {
    const coll = collection (db, 'todos')
    const querySnap = await getDocs (query(coll)) 
    todos.value = []
    querySnap.forEach((doc) => {
        let data = doc.data()
        data.id = doc.id
        todos.value.push(data)
    })
}

onMounted(() => { 
    getTodos ()
}) 

</script>
<template>

    <ul>
        <li v-for="todo in todos" key="todo.title">
        {{todo.title}}
        </li>
    </ul>
</template>