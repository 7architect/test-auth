<template>
  <div class="home">
    <auth-form v-on:form:success="redirect" />
  </div>
</template>

<script>
import AuthForm from "../components/auth-form";

export default {
  name: "Home",
  components: {
    AuthForm
  },

  methods: {
    redirect(event) {
      console.log(event);
      this.$router.push('/user')
    }
  }
};
</script>
