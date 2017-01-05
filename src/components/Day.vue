<template>
	<div class="v-cal-date" @click="onDayClick(date)" :date="date" :active="active" :events="events">
		<div class="v-cal-day">
			<slot></slot>
		</div>
		<div class="v-cal-day-events" v-for="event in events">
			{{event.title}}
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