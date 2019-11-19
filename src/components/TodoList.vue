<template>
  <div>
    <h1>todo</h1>
    <div class="card my-3" v-for="todo in todos" :key="todo.id">
      <div class="card-body d-flex justify-content-between">
        <span :class="{completed: todo.completed}" @click="updateTodo(todo)">{{todo.title}}</span>
        <span @click="deleteTodo(todo)">🗑️</span>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import jwtDecode from 'jwt-decode'

export default {
  name: 'TodoList',
  props: {
    todos: Array,
  },
  methods: {
    deleteTodo(todo){
      this.$session.start()
      const token = this.$session.get('jwt')
      const requestHeader = {
        headers: {
          Authorization: "JWT " + token
        }
      }
      axios.delete(`http://localhost:8000/api/v1/todos/${todo.id}/`, requestHeader)
      .then((res)=>{
        console.log(res)
        const targetTodo = this.todos.find(function(el){
          return el === todo
        })
        
        const idx = this.todos.indexOf(targetTodo)

        if (idx > -1) {
          this.todos.splice(idx, 1)
        }

      })
      .catch((e)=>{
        console.log(e)
      })
    },
    updateTodo(todo){
      this.$session.start()
      const token = this.$session.get('jwt')
      const decoded_token = jwtDecode(token)
      const user_id = decoded_token.user_id

      const requestHeader = {
        headers: {
          Authorization: "JWT " + token
        }
      }

      const requestForm = new FormData()
      requestForm.append('user', user_id)
      requestForm.append('title', todo.title)
      requestForm.append('completed', !todo.completed)

      axios.put(`http://localhost:8000/api/v1/todos/${todo.id}/`, requestForm, requestHeader)
      .then((res)=>{
        console.log(res)
        todo.completed = !todo.completed
      })
      .catch((e)=>{
        console.log(e)
      })
    },
    
    
  },
}
</script>

<style>
.completed {
  text-decoration: line-through;
  color: gray;
}
</style>