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
		name: "newNote",
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
				this.$emit('addNote', this.note);
			}
		}
	}
</script>

<style>
	.new-note{
		text-align: center;
	}
</style>