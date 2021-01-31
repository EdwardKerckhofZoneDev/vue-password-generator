<template>
  <div>
    <p>
      Made by <a href="https://zonedev.be" target="_blank">Edward Kerckhof</a> |
      <a href="https://github.com/EdwardKerckhofZoneDev" target="_blank"
        >Github</a
      >
      |
      <a
        href="https://www.linkedin.com/in/edward-kerckhof-728741183/"
        target="_blank"
        >LinkedIn</a
      >
    </p>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'AppFooter',
})
</script>

<style lang="scss" scoped>
div {
  position: absolute;
  bottom: 0;
  height: 100px;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;

  p {
    font-style: initial !important;
  }
}
</style>
