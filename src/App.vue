<template>
	<div class="wrapper">
		<div class="wrapper-content">

			<section>
				<div class="container">
					<h1>{{title}}</h1>
					<new-note @add-note="addNote" :note="note"/>
					<message v-if="message" :message="message"/>
					<div class="control" v-if="notes.length>0">
						<search v-if="notes.length>0"
						        :value="search"
						        placeholder="Find your note"
						        @search="search=$event"
						/>
						<a href="#"
						   @click.prevent="grid = true"
						   class="control__btn"
						   :class="{'control__btn--active':grid}"
						>
							<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" ><rect x="3" y="3" width="7" height="7"></rect><rect x="14" y="3" width="7" height="7"></rect><rect x="14" y="14" width="7" height="7"></rect><rect x="3" y="14" width="7" height="7"></rect></svg>
						</a>
						<a href="#"
						   @click.prevent="grid = false"
						   class="control__btn"
						   :class="{'control__btn--active':!grid}"
						>
							<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="8" y1="6" x2="21" y2="6"></line><line x1="8" y1="12" x2="21" y2="12"></line><line x1="8" y1="18" x2="21" y2="18"></line><line x1="3" y1="6" x2="3" y2="6"></line><line x1="3" y1="12" x2="3" y2="12"></line><line x1="3" y1="18" x2="3" y2="18"></line></svg>
						</a>
						<a href="#" @click.prevent="notes=[]" class="contol__link remove">Remove all notes</a>
					</div>
					<notes :notes="filteredNotes" @removeNote="removeNote" @editNote="editNote" @undoChanges="undoChanges" :grid="grid"/>
				</div><!-- /.container -->

			</section>

		</div>
	</div>
</template>

<script>
	import NewNote from '@/components/NewNote';
	import Message from '@/components/Message';
	import Search from '@/components/Search';
	import Notes from '@/components/Notes';

	export default {
		data() {
			return {
				title: 'Notes App',
				message: null,
				grid: true,
				search: '',
				note: {
					id: null,
					title: '',
					importance: 0,
					description: '',
					date: new Date(Date.now()).toLocaleString(),
					editMode: false
				},
				beforeEditingNote: null,
				notes: [
					{
						id: 1,
						importance: 0,
						title: 'First Note',
						description: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit. Dolores, enim eveniet fugit ipsum nobis possimus quas quod tenetur?',
						date: new Date(Date.now()).toLocaleString(),
						editMode: false
					},
					{
						id: 2,
						importance: 1,
						title: 'Second Note',
						description: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit.',
						date: new Date(Date.now()).toLocaleString(),
						editMode: false
					},
					{
						id: 3,
						importance: 2,
						title: 'Third Note',
						description: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit. Consectetur, cumque dicta dignissimos ducimus enim est et in maiores minima, molestiae placeat recusandae reiciendis repudiandae, similique ullam vero vitae?',
						date: new Date(Date.now()).toLocaleString(),
						editMode: false
					},
				],
			}
		},
		props: {},
		computed: {
			filteredNotes(){
				let arr = this.notes,
					search = this.search;

				if (!search) return arr;

				search = search.trim().toLowerCase();

				arr = arr.filter(
					(item)=>{
						return item.title
							.trim()
							.toLowerCase()
							.indexOf( search ) !== -1 ;
					}
				)
				return arr;
			}
		},
		methods: {
			addNote() {
				let {title, description, importance} = this.note;
				if (!title || !description) {
					this.message = "Fields can't be blank!";
					return
				} else this.message = null
				// массив уже отсортирован по убыванию
				let id = this.notes.length ? this.notes[0].id + 1 : 1;
				let date = new Date(Date.now()).toLocaleString();
				this.notes.push({id, title, importance, description, date, editMode: false});
				// сортируем по id, чтоб новые были вначале
				this.notes.sort((a, b) => a.id < b.id ? 1 : -1);
				this.note.title = '';
				this.note.importance = 0;
				this.note.description = '';
			},
			removeNote(id) {
				const index = this.notes.findIndex(n => n.id === id);
				if (index !== -1) {
					this.notes.splice(index, 1);
				}
			},
			editNote(id) {
				// проверка на случай уже открытого редактирования
				// пройдется по массиву, сбросит и закроет редактируемый
				if (this.beforeEditingNote){
					this.notes.forEach((note)=>{
						if (note.editMode) {
							note.title = this.beforeEditingNote.title;
							note.importance = this.beforeEditingNote.importance;
							note.description = this.beforeEditingNote.description;
							note.date = this.beforeEditingNote.date;
							note.editMode = false;
						}
					});
				}
				// определяем нужный нот
				const index = this.notes.findIndex(n => n.id === id);
				if (index !== -1) {
					// и сохраняем значение на случай отмены
					this.beforeEditingNote = {
						id: this.notes[index].id,
						title: this.notes[index].title,
						importance: this.notes[index].importance,
						description: this.notes[index].description,
						date: this.notes[index].date,
						editMode: false
					};
					// включаем у нота режим редактирования
					this.notes[index].editMode = true;
				}
			},
			undoChanges(id){
				const index = this.notes.findIndex(n => n.id === id);
				if (index !== -1) {
					this.notes.splice(index, 1, this.beforeEditingNote)
					this.beforeEditingNote = null;
				}
			}
		},
		components: {Message, NewNote, Notes, Search},
		created() {
			// сортируем по id, чтоб новые были вначале
			this.notes.sort((a, b) => a.id < b.id ? 1 : -1);
		}
	}
</script>

<style lang="scss">
	@import "@/assets/scss/utils/vars.scss";

	h1 {
		font-size: 28px;
		font-weight: 900;
		color: $primary-color;
		margin-bottom: 36px;
	}

	.control {
		margin-bottom: 30px;
		display: flex;
		justify-content: flex-end;
		align-items: center;
		&__btn{
			border: 2px solid $light-color;
			padding: 3px;
			border-radius: 6px;
		}
		&__link, &__btn {
			color: $light-color;
			display: inline-block;
			&:not(:last-child){
				margin-right: 20px;
			}
			&--active, &:hover{
				color: $primary-color;
				border-color: $primary-color;
			}
		}
	}

	.remove {
		color: $light-color;
		&:hover {
			color: $danger-color;
		}
	}
</style>
