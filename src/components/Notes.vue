<template>
	<ul class="notes" :class="{'notes--column':!grid}">
		<li class="note"
		    :class="{'note--full':!grid}"
		    v-for="(note, index) in notes"
		    :key="index"
		>
			<div class="note__inner">
				<div class="note__header">
					<h4>{{note.title}}</h4>
					<a href="#" @click.prevent="removeNote(index)" class="note__remove remove">x</a>
				</div>
				<p class="note__body">
					{{note.description}}
					<span class="note__date">{{note.date}}</span>
				</p>
			</div>
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
			},
			grid: {
				type: Boolean,
				required: true
			}
		},
		methods:{
			removeNote(index){
				this.$emit('removeNote', index)
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
		&--column{
			flex-direction: column;
		}
	}
	.note{
		flex-basis: 50%;
		//border: 1px solid gray;
		padding: 20px 40px;
		&__inner{
			border: 1px solid #DCDFE6;
			border-radius: 14px;
			padding: 20px 40px;
			height: 100%;
			transition: transform .25s ease, border-color .25s ease;
			&:hover{
				border-color: $primary-color;
				transform: translateY(-3px);
				box-shadow: 0 3px 3px rgba($light-color, .3);
			}
		}
		&__header{
			display: flex;
			justify-content: space-between;
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
		&--full{
			text-align: center;
			.note__inner{
				border-radius: 20px;
			}
			.note__header h4{
				flex-grow: 1;
			}
		}
	}
</style>