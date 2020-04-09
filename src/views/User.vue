<template>
  <div class="about">
    <h1>Welcome | <u>{{ user }}</u></h1>
    <a @click="logout" class="logout-link">
      Logout
    </a>
    <iframe src="https://giphy.com/embed/8vQSQ3cNXuDGo" width="100%" height="480" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/cat-moment-remember-8vQSQ3cNXuDGo">via GIPHY</a></p>
  </div>
</template>

<style scoped>
  .logout-link {
    display: block;
    margin-bottom: 1rem;
    font-weight: bold;
    text-decoration: underline;
    cursor: pointer;
  }
</style>

<script>
import Cookies from 'js-cookie'

export default {
  data() {
    return {
      user: Cookies.get('user')
    }
  },

  methods: {
    logout() {
      Cookies.remove('user')
      this.$router.push('/')
    }
  }
}
</script>
