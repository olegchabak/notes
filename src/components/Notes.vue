<template>
	<ul class="notes">
		<li class="note"
		    v-for="(note, index) in notes"
		    :key="index"
		>
			<div class="note__header">
				<h4>{{note.title}}</h4>
			</div>
			<p class="note__body">
				{{note.description}}
				<span class="note__date">{{note.date}}</span>
			</p>
		</li>
	</ul><!-- /.notes -->
</template>

<script>
	export default {
		name: "notes",
		props:{
			notes: {
				type: Array,
				required: true
			}
		}
	}
</script>

<style lang="scss">
	@import "@/assets/scss/utils/vars.scss";

	.notes{
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
		margin: 0 -40px;
	}
	.note{
		flex-basis: 50%;
		//border: 1px solid gray;
		padding: 20px 40px;
		&__header{
			color: $primary-color;
			font-size: 24px;
			margin-bottom: 10px;
		}
		&__body{
			color: $neutral-primary;
		}
		&__date{
			display: block;
			margin-top: 15px;
			color: $neutral-secondary;
			font-size: 14px;
		}
	}
</style>