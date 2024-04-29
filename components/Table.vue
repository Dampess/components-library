<template>
  <table class="table">
    <thead class="bg-gray-700">
      <tr>
        <th v-for="(column, index) in columns" :key="index" @click="sortTable(column.key)" class="px-4 py-2 cursor-pointer">
          <div class="flex items-center">
            {{ column.label }}
            <span>
              <svg v-if="sortOrder === 'asc'" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" class="h-4 w-4 inline">
                <path d="M7 14l5-5 5 5z" />
              </svg>
              <svg v-else xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" class="h-4 w-4 inline">
                <path d="M7 10l5 5 5-5z" />
              </svg>
            </span>
          </div>
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(row, rowIndex) in sortedRows" :key="rowIndex" :class="rowIndex % 2 === 0 ? 'bg-gray-100' : ''">
        <td v-for="(column, colIndex) in columns" :key="colIndex" class="px-4 py-2">{{ row[column.key] }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  props: {
    data: {
      type: Array,
      default: () => []
    },
    columns: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      sortColumn: '',
      sortOrder: 'asc'
    };
  },
  computed: {
    sortedRows() {
      if (!this.sortColumn) return this.data;

      const sortedData = [...this.data];

      sortedData.sort((a, b) => {
        const valueA = a[this.sortColumn];
        const valueB = b[this.sortColumn];

        if (valueA < valueB) {
          return this.sortOrder === 'asc' ? -1 : 1;
        }
        if (valueA > valueB) {
          return this.sortOrder === 'asc' ? 1 : -1;
        }
        return 0;
      });

      return sortedData;
    }
  },
  methods: {
    sortTable(columnKey) {
      if (this.sortColumn === columnKey) {
        this.sortOrder = this.sortOrder === 'asc' ? 'desc' : 'asc';
      } else {
        this.sortColumn = columnKey;
        this.sortOrder = 'asc';
      }
    }
  }
};
</script>

<style scoped>
.table {
  width: 100%;
  border-collapse: collapse;
}

.table th,
.table td {
  border: 1px solid #908f8f;
  padding: 8px;
}

.table th {
  cursor: pointer;
}

.table th:hover {
  background-color: #f2f2f2;
}
</style>
