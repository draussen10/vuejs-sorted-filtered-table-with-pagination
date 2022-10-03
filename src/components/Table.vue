<template>
  <table>
    <thead>
      <tr>
        <th>Дата</th>
        <th @click="setSortValue('name')">
          Название
          <i
            class="tiny material-icons"
            :class="[{ rotated: sortFlag }, { none: sortColumn !== 'name' }]"
            >arrow_downward</i
          >
        </th>
        <th @click="setSortValue('count')">
          Количество
          <i
            class="tiny material-icons"
            :class="[{ rotated: sortFlag }, { none: sortColumn !== 'count' }]"
            >arrow_downward</i
          >
        </th>
        <th @click="setSortValue('distance')">
          Расстояние
          <i
            class="tiny material-icons"
            :class="[
              { rotated: sortFlag },
              { none: sortColumn !== 'distance' },
            ]"
            >arrow_downward</i
          >
        </th>
      </tr>
    </thead>
    <tbody> 
			<TableRow 
				v-for="note in sortedAndFilteredAndPaginatedNotes" 
				:key="note.id"
				:note="note" />
    </tbody>
  </table>
</template>
 
<script>
import TableRow from "@/components/TableRow.vue";

export default {
  components: {
    TableRow,
  },
  props: {
    sortedAndFilteredAndPaginatedNotes: {
      type: Array,
      default: [],
      required: true,
    }
  },
  data: () => ({
    sortColumn: "",
    sortFlag: false,
  }),
  methods: {
    setSortValue(value) {
      if (this.sortColumn != value) {
        this.sortColumn = value;
        this.sortFlag = false;
      } else {
        if (!this.sortFlag) {
          this.sortFlag = true;
        } else {
          this.sortColumn = "";
          this.sortFlag = false;
        }
      }
			this.$emit('SortActivation', this.sortColumn, this.sortFlag)
    },
  }
};
</script>
 
<style scoped>
th {
  cursor: pointer;
  user-select: none;
	width: 25%;
}
th:nth-child(1) {
  cursor: auto;
}
.rotated {
  transform: rotate(180deg);
}
.none {
  display: none;
}
</style>