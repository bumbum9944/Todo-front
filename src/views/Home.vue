<template>
  <div class="home">
    <TodoInput @createTodo="createTodo"/>
    <TodoList v-bind:todos="todos"/>
  </div>
</template>

<script>
import router from '../router'
import TodoList from '../components/TodoList.vue'
import TodoInput from '@/components/TodoInput.vue'
// import jwtDecode from 'jwt-decode'
import axios from 'axios'
import { mapGetters } from 'vuex'

export default {
  name: 'home',
  components: {
    TodoList,
    TodoInput
  },
  data() {
    return {
      todos: []
    }
  },
  computed: {
    ...mapGetters([
      'isAuthenticated',
      'requestHeader',
      'userId'
    ])
  },
  methods: {
    checkLoggedIn(){
      // this.$session.start()
      if (!this.isAuthenticated){
        // 로그인 페이지로 redirect
        router.push('/login')
      }
    },
    getTodos(){
      // this.$session.start()
      // const token = this.$session.get('jwt')
      // const decoded_token = jwtDecode(token)
      // const user_id = decoded_token.user_id

      // const requestHeader = {
      //   headers: {
      //     Authorization: "JWT " + token
      //   }
      // }

      axios.get(`http://localhost:8000/api/v1/users/${this.user_id}/`, this.requestHeader)
        .then((res)=>{
          this.todos = res.data.todo_set
        })
        .catch((e)=>{
          console.log(e)
        })
    },
    createTodo(title){
      // this.$session.start()
      // const token = this.$session.get('jwt')
      // const decoded_token = jwtDecode(token)
      // const user_id = decoded_token.user_id

      // const requestHeader = {
      //   headers: {
      //     Authorization: "JWT " + token
      //   }
      // }

      const requestForm = new FormData()
      requestForm.append('user', this.user_id)
      requestForm.append('title', title)

      axios.post('http://localhost:8000/api/v1/todos/', requestForm, this.requestHeader)
      .then((res)=>{
        this.todos.push(res.data)
      })
      .catch((e)=>{
        console.log(e)
      })
    }
  },
  mounted: function(){
    this.checkLoggedIn()
    this.getTodos()
  },
}
</script>

<style>

</style>