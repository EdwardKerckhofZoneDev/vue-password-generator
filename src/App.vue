<template>
  <h1>Secure Password Generator</h1>
  <hr />
  <password-generator />
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import PasswordGenerator from './components/PasswordGenerator.vue'

export default defineComponent({
  components: { PasswordGenerator },
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
  color: $clr-primary;
  margin-top: 60px;

  hr {
    width: 30%;
    margin: 20px auto;
  }
}
</style>
