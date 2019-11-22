<template>
  <div id="app">
    <Dashboard />
  </div>
</template>

<script>
import Dashboard from '@/views/dashboard.vue'

export default {
  name: 'app',
  components: {
    Dashboard
  }
}
</script>

<style lang="scss">
@import '@/styles/index.scss';
</style>
