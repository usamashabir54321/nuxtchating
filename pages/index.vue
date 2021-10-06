<template>
	<div class="container">

		<div class="d-flex flex-column align-items-stretch flex-shrink-0 bg-white mt-5 border-top">
			<div class="list-group list-group-flush border-bottom scrollarea">
				<div v-for="(message , index) in messages" :key="index" class="list-group-item list-group-item-action py-3 lh-tight">
					<div class="d-flex w-100 align-items-center justify-content-between"><strong class="mb-1">{{ message.username }}</strong></div>
					<div class="col-10 mb-1 small">{{ message.message }}</div>
				</div>
			</div>
		</div>

		<form @submit.prevent="submit">
			<input v-model="addMsg.message" class="form-control" placeholder="Write a message"/>
			<input v-model="addMsg.username" class="form-control mt-2" placeholder="Write User Name" />
			<div class="text-center pt-2"><button type="submit" class="btn btn-info">Submit</button></div>
		</form>

	</div>
</template>

<script>
	import Pusher from 'pusher-js';
	import axios from 'axios';
	export default {
		data() {
			return {
				messages: [],
				addMsg: {},
			}
		},
		mounted() {
			Pusher.logToConsole = true;
			const pusher = new Pusher('999dcbf7efa57b953816', {
				cluster: 'eu'
			});
			const channel = pusher.subscribe('nuxtjs-chat');
			channel.bind('message', data => {
				this.messages.push(data);
			});
		},
		methods: {
			async submit() {
				await axios.post('http://127.0.0.1:8000/api/message', this.addMsg);
				this.message = '';
			}
		}
	}
</script>

<style>
	.scrollarea {
		min-height: 500px;
	}
</style>