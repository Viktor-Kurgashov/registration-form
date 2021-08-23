<template>
  <TheForm v-if="currentPage === 'form'"
    @show-notification="this.currentPage='notice'" />

  <RegistrationNotice v-else
    @return-to-form="this.currentPage = 'form'" />
</template>

<script>
import TheForm from './components/TheForm.vue'
import RegistrationNotice from './components/RegistrationNotice.vue'

export default {
  name: 'App',
  components: {
    TheForm,
    RegistrationNotice
  },

  data() { return { currentPage: 'form' } }
}
</script>

<style>
* {
  box-sizing: border-box
}
body {
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif ;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;

  font-size: 1.25em;
  max-width: 400px;
  margin: 0 auto;
  padding-top: 40px;
}
.clearfix::after {
  content: "";
  clear: both;
  display: table;
}
</style>