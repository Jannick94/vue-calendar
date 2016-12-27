<template>
  <div id="app">
    <calendar></calendar>
  </div>
</template>

<script>
import Calendar from './components/Calendar'

export default {
  name: 'app',
  components: {
    Calendar
  }
}
</script>

<style>
  html, body {
    height: 100%;
  }
</style>
