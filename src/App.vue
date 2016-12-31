<template>
  <div id="app">
    <calendar start="2016-01-05" end= "2016-12-26"></calendar>
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
