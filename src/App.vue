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
					<notes :notes="filteredNotes" @removeNote="removeNote" :grid="grid"/>
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
					title: '',
					description: '',
					date: new Date(Date.now()).toLocaleString()
				},
				notes: [
					{
						title: 'First Note',
						description: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit. Dolores, enim eveniet fugit ipsum nobis possimus quas quod tenetur?',
						date: new Date(Date.now()).toLocaleString()
					},
					{
						title: 'Second Note',
						description: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit.',
						date: new Date(Date.now()).toLocaleString()
					},
					{
						title: 'Third Note',
						description: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit. Consectetur, cumque dicta dignissimos ducimus enim est et in maiores minima, molestiae placeat recusandae reiciendis repudiandae, similique ullam vero vitae?',
						date: new Date(Date.now()).toLocaleString()
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
				let {title, description} = this.note;
				if (!title || !description) {
					this.message = "Fields can't be blank!";
					return
				} else this.message = null
				let date = new Date(Date.now()).toLocaleString();
				this.notes.push({title, description, date});
				this.note.title = '';
				this.note.description = '';
			},
			removeNote(index) {
				this.notes.splice(index, 1);
			},
		},
		components: {Message, NewNote, Notes, Search},
	}
</script>

<style lang="scss">
	@import "@/assets/scss/utils/vars.scss";

	h1 {
		font-size: 24px;
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
