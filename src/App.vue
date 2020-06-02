<template>
	<div class="wrapper">
		<div class="wrapper-content">

			<section>
				<div class="container">
					<h1>{{title}}</h1>
					<new-note @add-note="addNote" :note="note"/>
					<message v-if="message" :message="message"/>
					<notes :notes="notes"/>
				</div><!-- /.container -->

			</section>

		</div>
	</div>
</template>

<script>
	import Message from '@/components/Message';
	import NewNote from '@/components/NewNote';
	import Notes from '@/components/Notes';

	export default {
		data(){
			return {
				title: 'Notes App',
				message: null,
				note:{
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
		computed: {},
		methods: {
			addNote(){
				let { title, description } =  this.note;
				if (!title || !description) {
					this.message = "Fields can't be blank!";
					return
				} else this.message = null
				let date = new Date(Date.now()).toLocaleString();
				this.$emit('addNote', this.note)
				this.notes.push( { title, description, date } );
				this.note.title = '';
				this.note.description = '';
			}
		},
		components: {Message, NewNote, Notes}
	}
</script>

<style>
	h1{
		font-size: 24px;
	}
</style>
