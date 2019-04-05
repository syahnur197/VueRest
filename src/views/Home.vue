<template>
  <div class="home">
    <label for="">Email</label>
    <br>
    <input type="text" v-model="user.email">
    <br>
    <label for="">Password</label>
    <br>
    <input type="password" v-model="user.password">
    <br>
    <button @click="login">Login</button>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'home',
  components: {
    HelloWorld
  },
  data() {
    return {
      user: {
        email : '',
        password: ''
      }
    }
  },
  methods: {
    login() {
      Vue.axios.post('/auth/login', 
        {
          email: this.user.email,
          password: this.user.password
        }
      )
      .then(res => {
        console.log(res.data)
      })
    }
  }
}
</script>
