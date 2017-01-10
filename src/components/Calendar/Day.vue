<template>
	<div class="v-cal-date"
			:date="date"
			:active="active"
			:events="events"
		>
		<calendar-modal :events="events"></calendar-modal>
		<div class="v-cal-day" @click="onDayClick(date)">
			<slot></slot>
		</div>
		<div class="v-cal-day-events" v-if="events">
			<div class="v-cal-day-event"
					 v-for="event in events"
					 :style="{ backgroundColor: event.bgColor }"
			>
				{{ event.title }}
			</div>
			<small class="v-cal-event-show-more" v-if="events.length > 3">more</small>
		</div>
	</div>
</template>

<script>
	import CalendarModal from './CalendarModal';

	export default {
		name: 'day',
		components: {
			CalendarModal
		},
		props: ['date', 'active', 'events'],
		methods: {
			onDayClick(date) {
				if (this.active) {
					Event.$emit('dayClicked', date);

					if (date.events.length) {
						this.$children[0].toggleModal();
					}
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

      &.today {
      	.v-cal-day {
      		background-color: #FFEB3B;

					&:hover {
						background-color: #FDD835;
					}
      	}
      }

      &.day-disabled {
        color: #ddd;
      }

      .v-cal-day {
        border-radius: 50%;
        padding: 10px;
        transition: background-color .15s ease-in-out;
        cursor: pointer;

        &:hover {
        	background-color: #eee;
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