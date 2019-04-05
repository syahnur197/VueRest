<template>
  <div class="home">
    <div v-if="!loggedIn">
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
    <div v-else>
      <button @click="logout">Logout</button>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'home',
  data() {
    return {
      user: {
        email : '',
        password: ''
      },
      loggedIn : false
    }
  },
  methods: {
    login() {
      this.axios.post('/auth/login', 
        {
          email: this.user.email,
          password: this.user.password
        }
      )
      .then(res => {
        localStorage.setItem('token', JSON.stringify(res.data));
        this.loggedIn = true;
      })
    },
    logout() {
      let token = localStorage.getItem('token')
      token = JSON.parse(token)
      token = token.access_token
      this.axios.get('/auth/logout', {
        headers : {
          'Authorization' : 'Bearer ' + token
        }
      }).then(res => {
      })
      localStorage.removeItem('token')
      this.loggedIn = false;
    }
  },
  mounted() {
    if(!this.loggedIn) {
      if(localStorage.getItem('token') !== null) {
        // checking if user is legit
        let token = localStorage.getItem('token')
        token = JSON.parse(token)
        token = token.access_token
        this.axios.get('/auth/user', {
          headers: {
            'Authorization' : 'Bearer ' + token
          }
        }).
        then( res => {
          // if legit log the user in
          this.loggedIn = true
        })
      } else {
        this.loggedIn = false;
      }
    }
  }
}
</script>
