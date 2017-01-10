<template>
	<div class="v-cal-modal" :events="events" :class="{ 'is-active': modalActive }">
		<div class="v-cal-modal-backdrop" @click="toggleModal()"></div>
		<div class="v-cal-modal-content">
			<button type="button" class="v-cal-modal-close" @click="toggleModal()">&times;</button>
			<ul class="v-cal-modal-event-list" >
				<li class="v-cal-modal-event" v-for="event in events">
					<span class="v-cal-event-circle" :style="{ backgroundColor: event.bgColor }"></span><span>{{ event.title }}</span>
				</li>
			</ul>
		</div>
	</div>
</template>

<script>
	export default {
		name: 'calendar-modal',
		props: ['events'],
		data() {
			return {
				modalActive: false
			}
		},
		methods: {
			toggleModal() {
				this.modalActive = !this.modalActive;
			}
		}
	}
</script>

<style lang="scss" scoped>
	.v-cal-modal {
		position: fixed;
		top: 0px;
		left: 0;
		right: 0;
		bottom: 0;
		display: none;

		&.is-active {
			display: flex;
			flex-direction: column;
		}

		.v-cal-modal-backdrop {
			background: rgba(0,0,0,0.3);
			position: absolute;
			top: 0;
			left: 0;
			right: 0;
			bottom: 0;
		}

		.v-cal-modal-content {
			top: 100px;
			margin: 0 auto;
			position: relative;
			width: 640px;
			background-color: #fff;
	    border-radius: 5px;
	    box-shadow: 0 2px 3px rgba(10, 10, 10, 0.1), 0 0 0 1px rgba(10, 10, 10, 0.1);
	    padding: 20px;

	    .v-cal-modal-close {
	    	position: absolute;
	    	top: -10px;
	    	right: -10px;
	    	background-color: rgba(10, 10, 10, 0.7);
		    border: none;
		    border-radius: 50%;
		    cursor: pointer;
		    display: inline-block;
		    width: 30px;
		    height: 30px;
		    font-size: 22px;
		    text-align: center;
		   	color: #fff;
		   	outline: 0;
		   	line-height: 21px;

		   	&:hover {
		   		background-color: rgba(10, 10, 10, 0.8);
		   	}
	    }

	    .v-cal-modal-event-list {
	    	list-style: none;
	    	padding-left: 0;
	    	margin: 0;
	    	text-align: left;

	    	.v-cal-modal-event {
	    		display: flex;
	    		margin: 5px 0;
	    		line-height: 22px;
	    	}

	    	.v-cal-event-circle {
	    		width: 20px;
	    		height: 20px;
	    		background-color: #1976D2;
	    		margin-right: 5px;
	    		border-radius: 50%;
	    	}
	    }
		}
	}
</style>