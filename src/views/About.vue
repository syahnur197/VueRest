<template>
  <div class="about">
    <h1>This is all posts page</h1>
    <div style="border: 1px solid black; padding: 1em;">
      <label for="">title</label>
      <br>
      <input type="text" v-model="post.title">
      <br>
      <label for="">body</label>
      <br>
      <input type="text" v-model="post.body">
      <br>
      <button @click="addPost">Add Post</button>
    </div>
    <div>
      <div v-for="(post,index) in posts" :key="index">
        <h2>title: {{post.title}}</h2>
        <p>{{post.body}}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'about',
  data() {
    return {
      posts: [],
      post: {
        title: '',
        body: ''
      }
    }
  },
  methods: {
    loadPost() {
      let token = localStorage.getItem('token')
      // check if has token
      if (token !== null) {
        token = JSON.parse(token)
        token = token.access_token
        this.axios.get('/posts', {
          headers: {
            'Authorization' : 'Bearer ' + token
          }
        })
        .then(res => {
          this.posts = res.data
        })
      }
      
    },
    addPost() {
      let token = localStorage.getItem('token')
      if (token !== null) {
        token = JSON.parse(token)
        token = token.access_token
        this.axios.post('/posts', 
          {
            title: this.post.title,
            body: this.post.body,
          },
          {
            headers: {
              Authorization : 'Bearer ' + token
            }
          }
        )
        .then(res => {
          this.loadPost();
        })
      }
    }
  },
  mounted() {
    this.loadPost();
  }
}
</script>

