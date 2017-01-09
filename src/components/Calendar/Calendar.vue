<template>
  <div class="v-cal">
    <calendar-header></calendar-header>
    <div class="v-cal-body">
      <weekdays></weekdays>
      <div class="v-cal-dates">
        <day class="empty" v-for="firstEmptyDay in firstEmptyDays"></day>
        <day v-for="day in days"
             :date="day"
             :active="day.active"
             :class="{ 'day-disabled': !day.active, 'today': day.date.isSame(today, 'day') }"
             :events="day.events"
        >
          {{ day.date.format('DD') }}
        </day>
        <day class="empty" v-for="lastEmptyDay in lastEmptyDays"></day>
      </div>
    </div>
  </div>
</template>

<script>
import moment from 'moment';

import CalendarHeader from './Header';
import Weekdays       from './Weekdays';
import Day            from './Day';

export default {
  name: 'calendar',
  components: {
    Day,
    CalendarHeader,
    Weekdays
  },
  props: {
    start: {
      type: String
    },
    end: {
      type: String
    },
    events: {
      type: Array
    }
  },
  mounted() {
    this.calStart    = (this.start)? moment(this.start) : false;
    this.calEnd      = (this.end)? moment(this.end) : false;
    this.currentDate = this.getCurrentDate();
  },
  data () {
    return {
      currentDate: moment(),
      today: moment(),
      firstEmptyDays: [],
      lastEmptyDays: [],
      days: [],
      calStart: false,
      calEnd: false,
      showPrevMonth: true,
      showNextMonth: true
    }
  },
  watch: {
    currentDate: function(currentDate) {
      this.showPrevMonth  = this.shouldShowPrevMonth();
      this.showNextMonth  = this.shouldShowNextMonth();
      this.firstEmptyDays = this.getFirstDaysInMonth();
      this.lastEmptyDays  = this.getLastDaysInMonth();
      this.days           = this.getDaysInMonth();
      return moment(currentDate).daysInMonth();
    }
  },
  methods: {
    getCurrentDate() {
      let currentDate = moment();

      if (currentDate.isBefore(this.calStart)) {
        currentDate = this.calStart.clone();
      }

      if (currentDate.isAfter(this.calEnd)){
        currentDate = this.calEnd.clone();
      }

      return currentDate;
    },

    shouldShowPrevMonth() {
      if (this.currentDate.clone().subtract(1, 'months').endOf('month').isBefore(this.calStart)){
        return false;
      }

      return true;
    },

    shouldShowNextMonth() {
      if (this.currentDate.clone().add(1, 'months').startOf('month').isAfter(this.calEnd)){
        return false;
      }

      return true;
    },

    getDaysInMonth() {
      let startOfMonth  = moment(this.currentDate).clone().startOf('month');
      let endOfMonth    = moment(this.currentDate).clone().endOf('month');

      let days = [];
      let day = startOfMonth;

      while (day <= endOfMonth) {
          if (this.calStart && day.isBefore(this.calStart)) {
            days.push({ date: day, active: false, events: [] });
          } else if (this.calEnd && day.isAfter(this.calEnd)) {
            days.push({ date: day, active: false, events: [] });
          } else {
            let formatted = day.clone().format('YYYY-MM-DD');
            let events;

            if (this.events) {
              events = this.events.filter( (d) => {
                return d.date == formatted;
              });
            }

            days.push({ date: day, active: true, events });
          }

          day = day.clone().add(1, 'd');
      }

      return days;
    },

    getFirstDaysInMonth() {
      let firstDays    = [];
      let weekDay = this.currentDate.clone().startOf('month').isoWeekday();

      for (var i = 0; i < (weekDay - 1); i++) {
        firstDays.push(i);
      }

      return firstDays;
    },

    getLastDaysInMonth() {
      let lastDays    = [];
      let weekDay = this.currentDate.clone().endOf('month').isoWeekday();

      for (var i = 0; i < (7 - weekDay); i++) {
        lastDays.push(i);
      }

      return lastDays;
    }
  }
}
</script>

<style lang="scss">
  *:before, *:after, *  {
    box-sizing: border-box;
  }

  .v-cal {
    font-family: sans-serif;
    display: flex;
    width: 100%;
    flex-direction: column;

    .v-cal-body {
      margin-top: 20px;
    }

  }
</style>
