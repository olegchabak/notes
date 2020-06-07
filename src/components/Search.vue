<template>
	<div class="search">
		<input type="text" class="search__input" v-model="search" :placeholder="placeholder">
		<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-search search__icon"><circle cx="11" cy="11" r="8"></circle><line x1="21" y1="21" x2="16.65" y2="16.65"></line></svg>
	</div>
</template>

<script>
	export default {
		name: "Search",
		data(){
			return {
				search: this.value
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
		 }
		&__icon{
			position: absolute;
			left: 20px;
			top: 15px;
			color: $neutral-placeholder;
		}
	}
</style>