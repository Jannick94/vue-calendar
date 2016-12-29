<template>
  <div class="v-cal">
    <div class="v-cal-heading">
      <div class="v-cal-prev" @click="prevMonth()">prev</div>
      <div class="v-cal-date">
        {{ currentDate | moment("MMMM YYYY") }}
      </div>
      <div class="v-cal-next" @click="nextMonth()">next</div>
    </div>
    <div class="v-cal-body">
      <div class="v-cal-weekdays">
        <div class="v-cal-weekday" v-for="weekDay in weekDays">
          {{ weekDay }}
        </div>
      </div>

      <div class="v-cal-dates">
        <day v-for="firstEmptyDay in firstEmptyDays"></day>
        <day v-for="day in days"> {{ day | moment("DD") }} </day>
        <day v-for="lastEmptyDay in lastEmptyDays"></day>
      </div>
    </div>
  </div>
</template>

<script>
import moment from 'moment';
import Day from './Day';

export default {
  name: 'calendar',
  components: {
    Day
  },
  created() {
    moment.locale('nl');
  },
  mounted() {
    this.firstEmptyDays = this.getFirstDaysInMonth();
    this.lastEmptyDays  = this.getLastDaysInMonth();
    this.days           = this.getDaysInMonth();
  },
  data () {
    return {
      currentDate: moment(),
      daysInMonth: 0,
      firstEmptyDays: [],
      lastEmptyDays: [],
      days: [],
      weekDays: ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
    }
  },
  watch: {
    currentDate: function(currentDate) {
      this.firstEmptyDays = this.getFirstDaysInMonth();
      this.lastEmptyDays  = this.getLastDaysInMonth();
      this.days           = this.getDaysInMonth();
      return moment(currentDate).daysInMonth();
    }
  },
  methods: {
    nextMonth() {
      this.currentDate = moment(this.currentDate).add('1', 'month');
    },

    prevMonth() {
      this.currentDate = moment(this.currentDate).subtract('1', 'month');
    },

    getDaysInMonth() {
      let startOfMonth  = moment(this.currentDate).startOf('month');
      let endOfMonth    = moment(this.currentDate).endOf('month');

      let days = [];
      let day = startOfMonth;

      while (day <= endOfMonth) {
          days.push(day.toDate());
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
    border: 1px solid #000;
    display: flex;
    width: 100%;
    padding: 20px;
    flex-direction: column;

    .v-cal-heading {
      display: flex;
      flex: 1;
      justify-content: space-between;
    }

    .v-cal-body {
      margin-top: 20px;

      .v-cal-weekdays {
        display: flex;
        flex-wrap: wrap;

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
          border: 1px solid #000;
          padding: 10px;
        }
      }

    }

  }
</style>
