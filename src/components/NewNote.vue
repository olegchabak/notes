<template>
	<div class="new-note">
		<div class="new-note__header">
			<div class="new-note__title">
				<label for="newNoteInput">Title</label>
				<input v-model="note.title" type="text" id="newNoteInput" class="new-note__field">
			</div>
			<div class="new-note__importance">
				<label for="newNoteSelect">Importance</label>
				<select class="new-note__select" name="" id="newNoteSelect" v-model="note.importance">
					<option value="0">Default</option>
					<option value="1">Important</option>
					<option value="2">Highly important</option>
				</select>
			</div>
		</div>
		<label for="newNoteTextarea">Description</label>
		<textarea v-model="note.description" id="newNoteTextarea" class="new-note__field"></textarea>
		<div class="new-note__btn-wrap">
			<button @click.prevent="addNote" class="btn btnPrimary">New note</button>
		</div>
	</div>
</template>

<script>
	export default {
		name: "new-note",
		data(){
			return {
				counter: 0,
			}
		},
		props:{
			note: {
				type: Object,
				required: true,
			}
		},
		methods:{
			addNote(){
				this.$emit('add-note', this.note);
			}
		}
	}
</script>

<style lang="scss">
	@import "@/assets/scss/utils/vars.scss";
	.new-note{
		margin-bottom: 36px;
		&__header{
			display: flex;
		}
		&__title{
			flex-grow: 1;
		}
		&__importance{
			margin-left: 40px;
		}
		&__select{
			color: $neutral-regular;
		}
		&__btn-wrap{
			text-align: center;
		}
		&__field {
			display: block;
			margin-bottom: 30px;
		}
	}
</style>