<template>
  <div>
    <p>
      Scenario 1: If you click on "Auto filter name" and then
      Reset, it works. Then, uncoment the line 15, retry, it doesnt work. 
    </p>
    <p>
    Scenario 2: Resetting by "=null" (uncomment line 16),
    autofilter Name + Reset => it works. However, Autofilter Age + Reset does not work
    </p><br>
  <a href="#" @click.stop="autofilter('name')">Auto filter Name</a>&nbsp;&nbsp;
  <a href="#" @click.stop="autofilter('age')">Auto filter age</a>
  &nbsp;&nbsp;
  <a href="#" @click.stop="autofilter('reset')">Reset</a>

    <vue-good-table
      :columns="columns"
      :rows="rows"
      :search-options="{
        enabled: false,
      }"
      :pagination-options="{
        enabled: true,
        perPage: 2,
        setCurrentPage: 1,
      }">
    </vue-good-table>
    <h3>Grouped Table</h3>
    <grouped-table></grouped-table>
  </div>
</template>

<script>
import GroupedTable from './grouped-table';

export default {
  name: 'test',
  mounted() {
    // setTimeout(() => {
    //   this.testing = [50, 100, 300];
    // }, 5000);
  },
  data() {
    return {
      searchQuery: '',
      testing: [2, 7, 12],
      columns: [
        {
          label: 'Name',
          field: 'name',
          filterOptions: {
            enabled: true,
            filterValue: null,
          },
        },
        {
          label: 'Age',
          field: 'age',
          type: 'number',
          filterOptions: {
            enabled: true,
            filterDropdownItems: ['>30', '<=30'],
            filterFn: this.filterAge,
            filterValue: null,
          },
        },
        {
          label: 'Created On',
          field: 'createdAt',
          type: 'date',
          dateInputFormat: 'YYYY-MM-DD',
          dateOutputFormat: 'MMM Do YY',
        },
        {
          label: 'Percent',
          field: 'score',
          type: 'percentage',
        },
      ],
      rows:  [
      { id:1, name:"John", age: 20, createdAt: '201-10-31:9: 35 am',score: 0.03343 },
      { id:2, name:"Jane", age: 24, createdAt: '2011-10-31', score: 0.03343 },
      { id:3, name:"Susan", age: 16, createdAt: '2011-10-30', score: 0.03343 },
      { id:4, name:"Chris", age: 55, createdAt: '2011-10-11', score: 0.03343 },
      { id:5, name:"Dan", age: 40, createdAt: '2011-10-21', score: 0.03343 },
      { id:6, name:"John", age: 20, createdAt: '2011-10-31', score: 0.03343 },
      { id:7, name:"Jane", age: 24, createdAt: '2013-09-21' },
      { id:8, name:"Susan", age: 16, createdAt: '2013-10-31', score: 0.03343 },
    ],
    };
  },
  methods: {
    autofilter(type) {
      if (type == 'name') {
        this.columns[0].filterOptions.filterValue = 'John';
      }
      if (type == 'age') {
        this.columns[1].filterOptions.filterValue = '>30';
      }
      if (type == 'reset') {
        this.columns[0].filterOptions.filterValue = '';
        this.columns[1].filterOptions.filterValue = '';
        // this.columns[1].filterOptions.filterValue = null;
      }
    },

    filterAge(data, filterString) {
      if ((filterString === '>30') && (parseInt(data, 10) > 30)) {
        return true;
      }
      if ((filterString === '<=30') && (parseInt(data, 10) <= 30)) {
        return true
      }
      return false;
    },

    onClick() {
      console.log('clicked');
    },
    addRow() {
      this.rows.push({
        name: `Chris ${Math.random()}`,
        age: 20,
      });
    },
    editRow() {
      this.$set(this.rows[12], 'age', 300);
    },
    sortFn(x, y, col) {
      if (x < y) {
        return -1;
      }
      if (x > y) {
        return 1;
      }
      return 0;
    },
    formatAge(value) {
      return `lala${value}lala`;
    },
    addFilter() {
      this.$set(this.columns[2], 'filterValue', 'Jane');
      console.log(this.columns);
    },

    // events
    // ===================================================
    onPerPageChange(evt) {
      // { currentPage: 1, currentPerPage: 10, total: 5 }
      console.log('per-page-changed:');
      console.log(evt);
    },

    onPageChange(evt) {
      // { currentPage: 1, currentPerPage: 10, total: 5 }
      console.log('page-changed:');
      console.log(evt);
    },

    onColumnFilter(params) {
      // { currentPage: 1, currentPerPage: 10, total: 5 }
      console.log('on-column-filters:');
      console.log(params);
    },

    onSearch(params) {
      console.log('on-search:');
      console.log(params);
    },

    onSortChange(params) {
      console.log('on-sort-change:');
      console.log(params);
    },

    onRowClick(params) {
      console.log('on-row-click');
      console.log(params);
    },
  },
  components: {
    'grouped-table': GroupedTable,
  },
};
</script>

<style lang="css">
  .row-style{
    background-color: red;
  }
  *{
    font-family: 'Open Sans';
  }
</style>

