<template>
	<div class="wrapper">
		<div class="wrapper-content">

			<section>
				<div class="container">
					<h1>{{title}}</h1>
					<div class="new-note">
						<input v-model="note.title" type="text" ref="input">
						<textarea v-model="note.description" ref="textarea"></textarea>
						<button @click.prevent="addNote">New note</button>
						<p v-if="message" class="error-message">{{message}}</p>
					</div>
					<ul class="notes">
						<li class="note"
						    v-for="(note, index) in notes"
						    :key="index"
						>
							<div class="note__header">
								<h4>{{note.title}}</h4>
							</div>
							<p class="note__body">
								<span>{{note.description}}</span>
								<br>
								<span>{{note.date}}</span>
							</p>
						</li>
					</ul><!-- /.notes -->
				</div><!-- /.container -->

			</section>

		</div>
	</div>
</template>

<script>
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
					// {
					// 	title: 'First Note',
					// 	description: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit. Dolores, enim eveniet fugit ipsum nobis possimus quas quod tenetur?',
					// 	date: new Date(Date.now()).toLocaleString()
					// },
					// {
					// 	title: 'Second Note',
					// 	description: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit.',
					// 	date: new Date(Date.now()).toLocaleString()
					// },
					// {
					// 	title: 'Third Note',
					// 	description: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit. Consectetur, cumque dicta dignissimos ducimus enim est et in maiores minima, molestiae placeat recusandae reiciendis repudiandae, similique ullam vero vitae?',
					// 	date: new Date(Date.now()).toLocaleString()
					// },
				],
			}
		},
		props: {},
		computed: {},
		methods: {
			addNote(){
				let { title, description } =  this.note;
				if (!title || !description) {
					this.message = 'Have empty fields!'
					return
				} else this.message = null
				let date = new Date(Date.now()).toLocaleString();
				this.notes.push( { title, description, date } );
				this.note.title = '';
				this.note.description = '';
			}
		}
	}
</script>

<style>
</style>