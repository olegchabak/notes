<template>
	<div class="search">
		<input type="text" class="search__input" ref="searchInput" v-model="search" :placeholder="placeholder">
		<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 310 310" class="search__icon"><path d="M274 215A126 126 0 0095 37C54 78 47 140 77 189c0 0 2 3-1 6l-64 64c-13 13-16 31-5 42l2 2c11 12 29 9 42-4l64-64c3-3 7-1 7-1 48 29 110 23 152-19zm-155-23A93 93 0 11251 60a93 93 0 01-132 132z"/><path d="M127 118a13 13 0 01-12-18c17-41 65-61 107-43a13 13 0 01-10 24c-28-12-61 1-73 29-2 5-7 8-12 8z"/></svg>
		<a href="#" v-if="search" @click.prevent="clearSearch" class="search__icon">
			<svg class="search__clear-icon" xmlns="http://www.w3.org/2000/svg"  width="24" height="24" viewBox="0 0 310 310"><path d="M274 215A126 126 0 0095 37C54 78 47 140 77 189c0 0 2 3-1 6l-64 64c-13 13-16 31-5 42l2 2c11 12 29 9 42-4l64-64c3-3 7-1 7-1 48 29 110 23 152-19zm-155-23A93 93 0 11251 60a93 93 0 01-132 132z"/><path d="M233 148l-20-20s-2-2 0-4l20-20c4-4 4-10 0-14l-12-12c-4-4-10-4-14 0l-20 20c-2 2-4 0-4 0l-20-20c-4-4-10-4-14 0l-11 12c-4 4-4 10 0 14l19 20s2 2 0 4l-19 20c-4 4-4 10 0 14l11 12c4 4 10 4 14 0l20-20c2-2 4 0 4 0l20 20c4 4 10 4 14 0l12-12c4-4 4-10 0-14z"/></svg>
		</a>
	</div>
</template>

<script>
	export default {
		name: "Search",
		data(){
			return {
				search: this.value,
			}
		},
		props:{
			value: {
				type: String,
				required: true
			},
			placeholder:{
				type: String,
				default: ''
			}
		},
		methods:{
			clearSearch(){
				// this.$refs.searchInput.value = null;
				this.search = '';
			}
		},
		watch:{
			search(val){
				this.$emit('search', val);
			}
		}
	}
</script>

<style lang="scss">
	@import "@/assets/scss/utils/vars.scss";

	.search{
		margin-right: auto;
		position: relative;
		width: 40%;
		&__input{
			margin-bottom: 0;
			padding-left: 60px;
			&::placeholder{
				color: $neutral-placeholder;
			}
		 }
		&__icon{
			position: absolute;
			display: block;
			background-color: #fff;
			line-height: 1;
			left: 20px;
			top: 50%;
			transform: translateY(-50%);
			color: $neutral-placeholder;
			fill: currentColor;
		}
		&__clear{
			display: block;
			background-color: #fff;
			z-index: 1;
		}
		&__clear-icon{
			fill: $neutral-placeholder;
		}
	}
</style>