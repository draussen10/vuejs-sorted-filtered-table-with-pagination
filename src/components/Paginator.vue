<template>
<div class="pagination-panel center">
	<div 
		class="page"
		v-for="page of pages"
		:key="page"
		@click="pageClickHandler(page)"
		:class="{'active': page === pageNum}"
	> {{ page }} </div>
</div>
</template>
 
<script>
 export default {
	props: {
		pages: {
			type: Number,
			required: true
		},
		pageNum: {
			type: Number,
			required: true
		},
  },
	methods: {
		pageClickHandler(page) {
			this.$emit('paginateActivation', page)
		}
	}
 }
</script>
 
<style scoped>
		.pagination-panel{
		margin-top: 70px;
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
	}
	.page{
		display: flex;
		justify-content: center;
		align-items: center;
		width: 40px;
		height: 40px;
		padding: 8px;
		margin: 0 10px;
		border-radius: 50%;
		border: 2px solid #000;
		font-weight: bold;
		cursor: pointer;
	}
	.page.active{
		color: white;
		background-color: rgb(180, 129, 227);
		border: 2px solid rgb(180, 129, 227);
	}
</style>