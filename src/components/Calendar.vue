<template>
  <div class="v-cal">
    <div class="v-cal-heading">
      <button type="button" class="v-cal-prev" v-show="showPrevMonth" @click="prevMonth()">&larr;</button>
      <h4 class="v-cal-date">
        {{ currentDate.format('MMMM YYYY') }}
      </h4>
      <button type="button" class="v-cal-next" v-show="showNextMonth" @click="nextMonth()">&rarr;</button>
    </div>
    <div class="v-cal-body">
      <div class="v-cal-weekdays">
        <div class="v-cal-weekday" v-for="weekDay in weekDays">
          {{ weekDay }}
        </div>
      </div>

      <div class="v-cal-dates">
        <day class="empty" v-for="firstEmptyDay in firstEmptyDays"></day>
        <day v-for="day in days"
             :date="day.date.format('DD-MM-YYYY')"
             :active="day.active"
             :class="((!day.active) ? 'day-disabled' : '')"
             :events="day.events"
        >
          {{ ((day.date)? day.date.format('DD') : '') }}
        </day>
        <day class="empty" v-for="lastEmptyDay in lastEmptyDays"></day>
      </div>
    </div>
  </div>
</template>

<script>
import Vue    from 'vue';
import moment from 'moment';
import Day    from './Day';
import Lang   from './../lang/nl';

window.Event = new Vue({});

export default {
  name: 'calendar',
  components: {
    Day
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
    console.clear();

    this.calStart    = (this.start)? moment(this.start) : false;
    this.calEnd      = (this.end)? moment(this.end) : false;
    this.currentDate = this.getCurrentDate();
  },
  data () {
    return {
      currentDate: moment(),
      firstEmptyDays: [],
      lastEmptyDays: [],
      days: [],
      weekDays: Lang.days,
      calStart: false,
      calEnd: false,
      showPrevMonth: true,
      showNextMonth: true,
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
      var currentDate = moment();

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

    prevMonth() {
      if (this.shouldShowPrevMonth()){
        this.currentDate = moment(this.currentDate).subtract('1', 'month');
      }
    },

    nextMonth() {
      if (this.shouldShowNextMonth()){
        this.currentDate = moment(this.currentDate).add('1', 'month');
      }
    },

    getDaysInMonth() {
      let startOfMonth  = moment(this.currentDate).startOf('month');
      let endOfMonth    = moment(this.currentDate).endOf('month');

      let days = [];
      let day = startOfMonth;

      while (day <= endOfMonth) {
          if (this.calStart && day.isBefore(this.calStart)) {
            days.push({ date: day, active: false });
          } else if (this.calEnd && day.isAfter(this.calEnd)) {
            days.push({ date: day, active: false });
          } else {
            let formatted = day.clone().format('YYYY-MM-DD');
            let events = this.events.filter( (d) => {
              return d.date == formatted;
            });

            days.push({ date: day, active: true, events });
          }

          day = day.clone().add(1, 'd');
      }

      return days;
    },

    getFirstDaysInMonth() {
      let firstDays    = [];
      let weekDay = this.currentDate.startOf('month').isoWeekday();

      for (var i = 0; i < (weekDay - 1); i++) {
        firstDays.push(i);
      }

      return firstDays;
    },

    getLastDaysInMonth() {
      let lastDays    = [];
      let weekDay = this.currentDate.endOf('month').isoWeekday();

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

    .v-cal-heading {
      display: flex;
      flex: 1;
      justify-content: space-between;

      .v-cal-date {
        margin: auto;
      }

      .v-cal-prev, .v-cal-next {
        padding: 10px;
        font-size: 24px;
        align-self: center;
        border: 0;
        background-color: transparent;
        outline: 0;

        &:hover {
          background-color: #eee;
        }

        &:active {
          background-color: #ddd;
        }
      }
    }

    .v-cal-body {
      margin-top: 20px;

      .v-cal-weekdays {
        display: flex;
        flex-wrap: wrap;
        color: rgba(0,0,0,0.58);
        margin-bottom: 20px;

        .v-cal-weekday {
          flex-basis: 14.2857142857%;
          text-align: center;
        }
      }

      .v-cal-dates {
        display: flex;
        flex-wrap: wrap;

        .v-cal-date {
          flex-basis: 14.2857142857%;
          text-align: center;
          padding: 10px;
          height: 100px;
          display: flex;
          justify-content: flex-start;
          align-items: flex-start;
          user-select: none;
          flex-direction: column;

          &.day-disabled {
            color: #ddd;
          }

          &:hover:not(.empty):not(.day-disabled) {
            .v-cal-day {
              background-color: #eee;
              cursor: pointer;
            }
          }
          &:active:not(.empty) {
            .v-cal-day {
              background-color: #ddd;
            }
          }

          .v-cal-day {
            border-radius: 50%;
            padding: 10px;
            transition: background-color .15s ease-in-out;

            &.today {
              background-color: gold;
            }
          }

          .v-cal-day-events {

            .v-cal-day-event {
              font-size: 12px;
              background-color: #1976D2;
              padding: 2px 4px;
              border-radius: 4px;
              margin-bottom: 5px;
              color: #fff;
            }
          }
        }
      }

    }

  }
</style>
