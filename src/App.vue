<template>
  <TheForm />
</template>

<script>
import TheForm from './components/TheForm.vue'

export default {
  name: 'App',
  components: {
    TheForm
  }
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
}
.clearfix::after {
  content: "";
  clear: both;
  display: table;
}
</style>