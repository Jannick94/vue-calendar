<template>
	<div class="v-cal-heading">
    <button type="button" class="v-cal-prev" v-show="$parent.showPrevMonth" @click="prevMonth()">&larr;</button>
    <h4 class="v-cal-date">
      {{ $parent.currentDate.format('MMMM YYYY') }}
    </h4>
    <button type="button" class="v-cal-next" v-show="$parent.showNextMonth" @click="nextMonth()">&rarr;</button>
  </div>
</template>

<script>
	import moment from 'moment';

	export default {
		name: 'calendar-header',
		methods: {
			prevMonth() {
	      if (this.$parent.shouldShowPrevMonth()){
	        this.$parent.currentDate = moment(this.$parent.currentDate).subtract('1', 'month');
	      }
	    },

	    nextMonth() {
	      if (this.$parent.shouldShowNextMonth()){
	        this.$parent.currentDate = moment(this.$parent.currentDate).add('1', 'month');
	      }
	    },
		}
	}
</script>

<style lang="scss">
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
</style>