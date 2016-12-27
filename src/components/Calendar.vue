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
        <div class="v-cal-day">01</div>
      </div>
    </div>
  </div>
</template>

<script>
import moment from 'moment';

export default {
  name: 'calendar',
  mounted() {

  },
  data () {
    return {
      currentDate: moment(),
      weekDays: ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
    }
  },
  methods: {
    nextMonth() {
      this.currentDate = moment(this.currentDate).add('1', 'month');
    },

    prevMonth() {
      this.currentDate = moment(this.currentDate).subtract('1', 'month');
    },
  }
}
</script>

<style lang="scss" scoped>
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
      }

    }

  }
</style>
