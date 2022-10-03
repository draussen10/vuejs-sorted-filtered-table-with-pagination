<template>
<div class="row">
	<div 
		class="input-field col s3"
	>
		<select
			v-model="columnFilter"
			class="browser-default"
		>
			<option value='' disabled selected>Выберите...</option>
			<option value="date">Дата</option>
			<option value="name">Название</option>
			<option value="count">Количество</option>
			<option value="distance">Расстояние</option>
		</select>
		<label class="select-label">Поле фильтрации</label>
	</div>

	<div class="input-field col s3"
	>
		<select
			v-model="conditionFilter"
			class="browser-default"
		>
			<option value='' disabled selected>Выберите...</option>
			<option value="==">Равно</option>
			<option value="contains" :disabled="columnFilter == 'count' || columnFilter == 'distance' || columnFilter == 'date'">Содержит</option>
			<option value=">" :disabled="columnFilter == 'name'">Больше</option>
			<option value="<" :disabled="columnFilter == 'name'">Меньше</option>
		</select>
		<label class="select-label">Условие фильтрации</label>
	</div>

	<div class="input-field col s3">
		<input
			placeholder="Введите значение"  
			:type=" columnFilter == 'date' ? 'date' : 'text' " 
			v-model="inputFilter" 
			:disabled="!columnFilter || !conditionFilter"
		>
		<label class="select-input">Значение</label>
	</div>

	<a v-if="columnFilter || conditionFilter" class="waves-effect waves-light red btn col s3" @click="clear">Очистить</a>

</div>
</template>
 
<script>
 export default {
	data: () => ({
		columnFilter: '',
		conditionFilter: '',
		inputFilter : ''
	}),
	methods: {
		clear () {
			this.columnFilter = '',
			this.conditionFilter = '',
			this.inputFilter = ''
		},
		createFilterConditions() {
			const filterConditions = {columnFilter: this.columnFilter, 
																conditionFilter: this.conditionFilter, 
																inputFilter: this.inputFilter}
			this.$emit('filterActivation', filterConditions)
		}
	},
	watch: {
		columnFilter() {
			this.conditionFilter = '',
			this.inputFilter = ''
		},
		conditionFilter() {
			this.createFilterConditions()
		},
		inputFilter() {
			this.createFilterConditions()
		}
	}
 }

</script>
 
<style scoped>
	.select-label {
		transform: translateY(-30px) !important;
	}
	.select-input{
		font-weight: 400;
		transform: translateY(-29px) !important;
	}
</style>