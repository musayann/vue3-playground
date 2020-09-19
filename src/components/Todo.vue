<template>

<div class="bg-gray-200 w-full min-h-screen py-24">
  <div class="w-1/2 bg-white shadow mx-auto py-8 px-6 rounded">
      <h1 class='text-2xl font-medium text-gray-700'>{{title}}</h1>
      <div class="my-3">
        <input v-model="state.keyword" @keydown.enter="addTodo" class="py-4 px-3 m-0 text-lg font-light border-gray-400 w-full border border-solid rounded outline-none focus:outline-none" placeholder="What needs to be done?"/>
      </div>
      <div v-for="(todo, index) in state.todos" :key="todo.id" class="py-3 flex items-center">
        <div><input type="checkbox" v-model="todo.isComplete" /></div>
        <div :class="{'line-through': todo.isComplete}" class="mx-6 text-xl">{{todo.description}}</div>
        <div @click="deleteTodo(index)" class="text-lg ml-auto mr-1 cursor-pointer">&times;</div>
      </div>
      <div class="text-gray-600 mt-4 font-light">{{itemsRemaining}} item(s) remaining</div>
  </div>

</div>

</template>

<script>
import {reactive, computed, onMounted, watch} from 'vue'

export default {
  name: "TodoApp",
  props: ['title'],
  setup(props){
      const state = reactive({
        todoId: 2,
        keyword: '',
        todos: [
          {
            id: 1,
            description: 'Finish the project deployment',
            isComplete: false
          }
        ]
      })

      function addTodo(){
        if(state.keyword){
          state.todos.push({
            id: state.todoId++,
            description: state.keyword,
            isComplete: false
          })
          state.keyword = null;
        }
      }

      function deleteTodo(index){
        state.todos.splice(index, 1)
      }

      const itemsRemaining = computed(() => state.todos.filter(todo => !todo.isComplete).length)

      onMounted(()=>{
        console.log(`welcome to ${props.title}`)
      })

      watch(
        () => state.todoId,
        (newValue, oldValue)=> {
          console.log('new-value: '+ newValue);
          console.log('old-value: '+ oldValue);
        }
      )

      return {
        state,
        addTodo,
        deleteTodo,
        itemsRemaining
      }
  }
}
