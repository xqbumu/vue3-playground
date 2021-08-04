<template>
  <img v-if="false" alt="Vue logo" src="./assets/logo.png" />
  <QuotaSelect msg="Hello Vue 3 + TypeScript + Vite" />
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import QuotaSelect from './components/QuotaSelect.vue'

export default defineComponent({
  name: 'App',
  components: {
    QuotaSelect
  }
})
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
