<template>
	<div class="new-note">
		<label for="newNoteInput">Title</label>
		<input v-model="note.title" type="text" id="newNoteInput">
		<label for="newNoteTextarea">Description</label>
		<textarea v-model="note.description" id="newNoteTextarea"></textarea>
		<button @click.prevent="addNote" class="btn btnPrimary">New note</button>
	</div>
</template>

<script>
	export default {
		name: "new-note",
		data(){
			return {}
		},
		props:{
			note: {
				type: Object,
				required: true
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
	.new-note{
		text-align: center;
		margin-bottom: 30px;
	}
	input, textarea {
		display: block;
		margin-bottom: 30px;
	}
</style>