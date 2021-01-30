<template>
  <h1>Secure Password Generator</h1>
  <hr />
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'App',
})
</script>

<style lang="scss">
@import './assets/scss/_varbiales.scss';

#app {
  height: 100%;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  line-height: 1.4;
  letter-spacing: 0.5px;
  color: #2c3e50;
  margin-top: 60px;

  h1 {
    margin-bottom: 30px;
  }

  hr {
    width: 30%;
    margin: 0 auto;
  }
}
</style>
