<template>
  <div class="home">
    <h1>Todos</h1>
  </div>
</template>

<script>
import router from '../router'
export default {
  name: 'home',
  components: {

  },
  methods: {
    checkLoggedIn(){
      this.$session.start()
      if (!this.$session.has('jwt')){
        // 로그인 페이지로 redirect
        router.push('/login')
      }
    }
  },
  mounted: function(){
    this.checkLoggedIn()
  }
}
</script>

<style>

</style>