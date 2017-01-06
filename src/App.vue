<template>
  <div id="app">
    <calendar start="2016-01-05" end="2016-12-26" :events="events"></calendar>
  </div>
</template>

<script>
import Calendar from './components/Calendar'

export default {
  name: 'app',
  components: {
    Calendar
  },
  data() {
    return {
      events: [
        { title: 'Test Event', date: '2016-12-20', bgColor: '#f00' },
        { title: 'Test Event 2', date: '2016-12-20' },
        { title: 'Test Event', date: '2016-12-26' },
        { title: 'Test Event 2', date: '2016-12-25' },
        { title: 'Test Event 3', date: '2016-12-22' },
        { title: 'Test Event 3', date: '2016-12-08' },
        { title: 'Test Event 3', date: '2016-11-22' },
      ]
    }
  },
  created() {
    Event.$on('dayClicked', (date) => this.onDayClick(date));
  },
  methods: {
    onDayClick(date) {
      console.log(date);
    }
  }
}
</script>

<style>
  html, body {
    height: 100%;
  }
</style>
