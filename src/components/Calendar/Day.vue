<template>
	<div class="v-cal-date" @click="onDayClick(date)" :date="date" :active="active" :events="events">
		<div class="v-cal-day">
			<slot></slot>
		</div>
		<div class="v-cal-day-events">
			<div class="v-cal-day-event" v-for="event in events" :style="{ backgroundColor: event.bgColor }">{{ event.title }}</div>
		</div>
	</div>
</template>

<script>
	export default {
		name: 'day',
		props: ['date', 'active', 'events'],
		methods: {
			onDayClick(date) {
				if (this.active) {
					Event.$emit('dayClicked', date);
				}
			}
		}
	}
</script>

<style lang="scss">
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
</style>