<template>
	<tr>
		<td>{{ note.date }}</td>
		<td>{{ note.name }}</td>
		<td>{{ note.count }}</td>
		<td>{{ note.distance }}</td>
	</tr>
</template>
 
<script>
 export default {
	props: {
		note: {
			type: Object,
			default: {},
			required: true,
		}
	},
 }
</script>
 
<style scoped>
	
</style>