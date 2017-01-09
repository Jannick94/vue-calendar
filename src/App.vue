<template>
  <div id="app">
    <calendar start="2017-01-05" end="2017-12-26" :events="events" @dayClick="onDayClick(day)"></calendar>
  </div>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      events: [
        { title: 'Test Event', date: '2017-01-20', bgColor: 'green' },
        { title: 'Test Event 2', date: '2017-01-20' },
        { title: 'Test Event', date: '2017-01-26' },
        { title: 'Test Event 2', date: '2017-01-25' },
        { title: 'Test Event 3', date: '2017-01-22' },
        { title: 'Test Event 3', date: '2017-01-08', bgColor: '#000' },
        { title: 'Test Event 3', date: '2017-11-22' },
      ]
    }
  },
  created() {
    Event.$on('dayClicked', (day) => this.onDayClick(day));
  },
  methods: {
    onDayClick(day) {
      console.log(day.events);
    }
  }
}
</script>

<style>
  html, body {
    height: 100%;
    margin: 0;
  }
</style>
