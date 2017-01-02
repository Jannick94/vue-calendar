<template>
	<div class="v-cal-date" @click="onDayClick(date)" :date="date">
		<div class="v-cal-day"><slot></slot></div>
	</div>
</template>

<script>
	export default {
		name: 'day',
		props: ['date'],
		methods: {
			onDayClick(date) {
				console.log('Clicked on day');
			}
		}
	}
</script>