<template>
  <h1>Password Generator</h1>
  <hr />
  <password-generator />
  <app-footer />
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import PasswordGenerator from './components/PasswordGenerator.vue'
import AppFooter from './components/Footer.vue'

export default defineComponent({
  name: 'App',
  components: { PasswordGenerator, AppFooter },
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
    margin: 50px auto;
  }
}
</style>
