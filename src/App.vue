<template>
  <div id="app" class="container">
		<Loader v-if="loading"/>
		<div  v-if="!loading">
			<Filtration 
			@filterActivation="filterActivation"
			/>

			<Table v-if="sortedAndFilteredAndPaginatedNotes.length"
			:sortedAndFilteredAndPaginatedNotes="sortedAndFilteredAndPaginatedNotes" 
			@SortActivation='SortActivation'
			/>

			<p class="center" v-else>Записей не найдено!</p>

			<Paginator 
			:pages="pages"
			:pageNum="pageNum"
			@paginateActivation="paginateActivation"
			/>
		</div>
		
  </div>
</template>

<script>
import Table from '@/components/Table'
import Filtration from '@/components/Filtration.vue'
import Paginator from '@/components/Paginator.vue'
import Loader from '@/components/Loader.vue'

export default {

  name: 'App',
  components: {
		Table, Filtration, Paginator, Loader
  },
	data: () => ({
		notes : [
			{"id":1,"date":"2022-10-10","name":"Владимир","count":100,"distance":100},
			{"id":2,"date":"2020-06-30","name":"Алексей","count":2000,"distance":10},
			{"id":3,"date":"2000-01-03","name":"Алексей","count":111,"distance":110},
			{"id":4,"date":"2001-02-11","name":"Семен","count":20,"distance":1110},
			{"id":5,"date":"2002-03-12","name":"Евгений","count":123,"distance":11230}
		],
		filterConditions: {columnFilter: '', 
												conditionFilter: '', 
												inputFilter: ''},

		sortColumn: '',
		sortFlag: false,

		pageNum: 1,
		pages: 1,
		notesPerPage: 2,

		loading: true
	}),
	methods: {
		SortActivation(sortColumn, sortFlag) {
			this.sortColumn = sortColumn
			this.sortFlag = sortFlag
		},
		filterActivation(filterConditions) {
			this.filterConditions = filterConditions
		},
		paginateActivation(page) {
			this.pageNum = page
		},
		isEmpty(filterConditions) {
			return Object.values(this.filterConditions).some(x => x === null || x === '')
		} 
	},
	mounted() {
		setTimeout( () => {
			this.notes = [
			{"id":1,"date":"2022-10-10","name":"Владимир","count":100,"distance":100},
			{"id":2,"date":"2020-06-30","name":"Алексей","count":2000,"distance":10},
			{"id":3,"date":"2000-01-03","name":"Алексей","count":111,"distance":110},
			{"id":4,"date":"2001-02-11","name":"Семен","count":20,"distance":1110},
			{"id":5,"date":"2002-03-12","name":"Евгений","count":123,"distance":11230}
			]
			this.loading = false
		}, 3000)
		
		this.pages =  Math.ceil(this.sortedAndFilteredNotes.length / this.notesPerPage)
	},
	computed: {
		filteredNotes() {
			if(this.isEmpty(this.filterConditions)){
				return this.notes
			} else {

				const filtered = (item) => {
					if(this.filterConditions.conditionFilter === '=='){
						return item[this.filterConditions.columnFilter] == this.filterConditions.inputFilter
					} else if(this.filterConditions.conditionFilter === '>') {
						return item[this.filterConditions.columnFilter] > this.filterConditions.inputFilter
					}
					else if(this.filterConditions.conditionFilter === '<') {
						return item[this.filterConditions.columnFilter] < this.filterConditions.inputFilter
					}
					else if(this.filterConditions.conditionFilter === 'contains') {
						return item[this.filterConditions.columnFilter].toLowerCase().includes(this.filterConditions.inputFilter.toLowerCase())
					} else{
						return item
					}
				}

				return this.notes.filter(filtered)
			}
		},
    sortedAndFilteredNotes() {
		  if (this.sortColumn === "") {
        return this.filteredNotes.sort((a, b) => a.id - b.id);
      } else if (this.sortColumn === "name") {
        return this.filteredNotes.sort((a, b) => {
          a = a.name.toLowerCase();
          b = b.name.toLowerCase();
          if (a < b) {
            return !this.sortFlag ? -1 : 1;
          }
          if (a > b) {
            return !this.sortFlag ? 1 : -1;
          }
          return 0;
        });
      } else {
        const compareNumbers = (a, b) => {
          a = a[this.sortColumn];
          b = b[this.sortColumn];
          return !this.sortFlag ? a - b : b - a;
        };
        return this.filteredNotes.sort(compareNumbers);
      }
    },
		sortedAndFilteredAndPaginatedNotes () {
			let from = (this.pageNum - 1) * this.notesPerPage
			let to = from + this.notesPerPage
			return this.sortedAndFilteredNotes.slice(from, to)
		},
  },
	watch: {
		sortedAndFilteredNotes() {
			this.pages =  Math.ceil(this.sortedAndFilteredNotes.length / this.notesPerPage)
		},
		filterConditions() {
			if(!this.isEmpty(this.filterConditions)){
				this.pageNum = 1
			}
		}
	}
}
</script>

<style>
	@import 'materialize-css/dist/css/materialize.css';

	.container {
		margin-top: 100px;
	}
</style>
