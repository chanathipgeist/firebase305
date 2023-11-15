<script setup>
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'
import ToDo from './components/ToDo.vue'
import TodoInput from './components/TodoInput.vue'
import {query, collection, getDocs,doc,updateDoc,deleteField, getDoc, deleteDoc } from 'firebase/firestore'
import { ref, onMounted } from 'vue';
import db from './firebase/init.js'

const todos = ref({})

    async function getTodos () {
    const coll = collection (db, 'todos')
    const querySnap = await getDocs (query(coll)) 
    todos.value = []
    querySnap.forEach((doc) => {
        let data = doc.data()
        data.id = doc.id
        todos.value.push(data)
        console.log(data)
    })
}

const updateTodo=()=>{
  getTodos()
}


async function toggleComplete(index){
  const docRef=doc(db,"todos",todos.value[index].id)
  const item= await getDoc(docRef)
  const data= item.data()
  await updateDoc(docRef,{'completed':!data.completed})
  await getTodos()
  console.log(todos.value[index].completed)
  console.log(docRef.value)
}

async function deleteTodo(index){
  console.log(todos.value[index])
  await deleteDoc(doc(db,"todos",todos.value[index].id))
  console.log("delete"+ index)
  getTodos()
}

onMounted(() => { 
    getTodos ()
}) 

</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />
    </div>
  </header>
    <!-- <TheWelcome /> -->
    <TodoInput  @update-todo="updateTodo"/>
    <ToDo :todos="todos" @toggleComplete="toggleComplete" @deleteTodo="deleteTodo"/>
  
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
