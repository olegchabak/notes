<template>
	<ul class="notes" :class="{'notes--column':!grid}">
		<li class="note"
		    :class="{'note--full':!grid}"
		    v-for="(note, index) in notes"
		    :key="note.id"
		    @dblclick="editNote(note.id)"
		    @keyup.esc="undoChanges(note.id)"
		>
			<div class="note__inner" :class="{'note__inner--edit': note.editMode}">
				<div class="note__header">
					<img src="../assets/img/alert.svg" alt="Important note" class="note__header-icon" v-if="+note.importance === 1 && !note.editMode">
					<img src="../assets/img/fire.svg" alt="Highly important note" class="note__header-icon" v-if="+note.importance === 2 && !note.editMode">
					<h4 class="note__header-title" title="Double click to edit" v-if="!note.editMode">{{note.title}}</h4>
					<select v-if="note.editMode" class="note__select" name="importance" v-model="note.importance">
						<option value="0">Default</option>
						<option value="1">Important</option>
						<option value="2">Highly important</option>
					</select>
					<div class="note__editor">
						<a href="#" @click.prevent="undoChanges(note.id)" class="note__edit" title="Undo changes" v-if="note.editMode">
							<svg class="note__edit-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><path d="M511 286c-9-91-91-158-183-158H192V48a16 16 0 00-26-12L6 164a16 16 0 000 25l160 128a16 16 0 0017 1c5-2 9-8 9-14v-80h139c42 0 80 30 85 72 4 48-33 88-80 88H208c-9 0-16 7-16 16v64c0 9 7 16 16 16h128c103 0 185-89 175-194z"/></svg>
						</a>
						<a href="#" @click.prevent="editNote(note.id)" class="note__edit" title="Edit note" v-else>
							<svg class="note__edit-icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><path d="M148 385l226-226 53 53-226 226zM29 423L0 512l89-29zM74 310L300 85l53 53-226 225zM321 64l64-64 127 127-64 64zM57 336l-18 54 83 83 54-18z"/></svg>
						</a>
						<a href="#" @click.prevent="removeNote(note.id)" class="note__remove" title="Remove note">
							<svg class="note__edit-icon" xmlns="http://www.w3.org/2000/svg" viewBox="-57 0 512 512"><path d="M156 31h86v14h31V29c0-16-13-29-29-29h-90c-16 0-29 13-29 29v16h31zm0 0M344 168H54c-8 0-14 7-13 14l24 300c1 17 15 30 32 30h204c17 0 31-13 33-30l24-300c0-7-6-14-14-14zM124 480h-1c-8 0-15-6-15-14L93 219a15 15 0 0131-2l15 247c0 8-6 15-15 16zm91-15a15 15 0 01-31 0V218a15 15 0 1131 0zm91-246l-15 246a15 15 0 11-31-1l15-247a15 15 0 1131 2zm0 0M398 120l-11-30c-2-8-10-14-18-14H29c-8 0-16 6-18 14L1 120a13 13 0 0012 17h373l6-2c5-3 8-9 6-15zm0 0"/></svg>
						</a>
					</div>
				</div>
				<input ref="editInput" type="text" v-if="note.editMode" v-model="note.title">
				<p class="note__body" title="Double click to edit" v-if="!note.editMode">
					{{note.description}}
					<span class="note__date">{{note.date}}</span>
				</p>
				<textarea v-model="note.description" v-if="note.editMode"></textarea>
				<div class="note__btn-wrap">
					<button class="note__btn btn btnPrimary btnSmall" @click="endOfEditing(note.id)" v-if="note.editMode">SAVE</button>
				</div>
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
		data(){
			return {

			}
		},
		methods:{
			removeNote(id){
				this.$emit('removeNote', id)
			},
			editNote(id){
				this.$emit('editNote', id)
			},
			endOfEditing(id){
				const index = this.notes.findIndex(n => n.id === id);
				if (index !== -1) {
					this.notes[index].editMode = false;
					this.notes[index].date = new Date(Date.now()).toLocaleString();
				}


			},
			undoChanges(id){
				this.$emit('undoChanges', id);
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
			&:hover, &--edit{
				border-color: rgba($primary-color, .5);
				transform: translateY(-3px);
				box-shadow: 0 3px 3px rgba($light-color, .3);
				.note__remove, .note__edit{
					opacity: 1;
				}
			}
		}
		&__header{
			display: flex;
			justify-content: space-between;
			align-items: center;
			color: $primary-color;
			font-size: 24px;
			margin-bottom: 30px;
		}
		&__header-title{
			flex-grow: 1;
		}
		&__header-icon{
			width: 28px;
			height: 28px;
			margin-right: 18px;
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
		&__remove, &__edit{
			font-weight: bold;
			padding: 0 10px;
			opacity: 0;
			color: $neutral-placeholder;
			transition: all 0.25s ease;
			&:hover{
				color: $danger-color;
			}
		}
		&__remove{

		}
		&__edit{
			margin-left: 10px;

		}
		&__edit-icon{
			width: 17px;
			height: 17px;
			fill: currentColor;
		}
		&__btn-wrap{
			text-align: center;
			margin-top: 30px;
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
		select{
			margin-bottom: 0;
			width: auto;
			font-size: 14px;
		}
		input, textarea {
			font-size: 14px;
		}
	}
</style>