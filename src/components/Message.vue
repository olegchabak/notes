<template>
	<div class="message">
		<p>{{message}}</p>
	</div>
</template>

<script>
	export default {
		name: "message",
		props: {
			message: {
				type: String,
				required: true
			}
		}
	}
</script>

<style scoped>
	.message{
		color: red;
		text-align: center;
	}
</style>