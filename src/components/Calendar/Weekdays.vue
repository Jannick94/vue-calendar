<template>
	<div class="v-cal-weekdays">
	  <div class="v-cal-weekday" v-for="weekDay in weekDays">
	    {{ weekDay }}
	  </div>
	</div>
</template>

<script>
	import Lang from './../../lang/nl';

	export default {
		name: 'weekdays',
		data() {
			return {
				weekDays: Lang.days
			}
		}
	}
</script>

<style lang="scss">
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
</style>