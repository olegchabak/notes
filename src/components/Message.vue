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

<style lang="scss">
	@import "@/assets/scss/utils/vars.scss";
	.message{
		color: $danger-color;
		text-align: center;
	}
</style>