<template>

  <q-header elevated>
        <q-toolbar>
          <q-avatar>
            <q-btn to="/" flat round dense icon="arrow_back" />
          </q-avatar>

          <q-toolbar-title>
            Reports
          </q-toolbar-title>
          </q-toolbar>
  </q-header>
  <q-page class="flex flex-center">
     <div class="q-pa-md">
    <q-table
      title="Reports"
      :rows="rows"
      :columns="columns"
      row-key="name"
      hide-bottom
    >
    <template v-slot:top-right>
        <q-btn
          color="deep-orange"
          icon-right="archive"
          glossy label="Export to Excel"
          no-caps
          @click="exportTable"
        />
      </template>
    </q-table>

  </div>
  </q-page>

</template>

<script>
import { exportFile, useQuasar } from 'quasar'
const columns = [
  {
    name: 'name',
    required: true,
    label: 'Book ID',
    align: 'left',
    field: row => row.name,
    format: val => `${val}`,
    sortable: true,

  },
  { name: 'Book_Name', align: 'center', label: 'Book Name', field: 'Book_Name', sortable: true },
  { name: 'Category', label: 'Category', field: 'Category', sortable: true },
  { name: 'Price', label: 'Price', field: 'Price' , sortable: true },
]

const rows = [
  {
    name: '1',
    Book_Name: 'Challenging Times',
    Category: 'Business',
    Price: '125.60',
  },
  {
    name: '2',
    Book_Name: 'Learning JavaScript',
    Category: 'Programming',
    Price: '56.00',
  },
  {
    name: '3',
    Book_Name: 'Popular Science',
    Category: 'Science',
    Price:  '210.40',
  },

]
function wrapCsvValue (val, formatFn) {
  let formatted = formatFn !== void 0
    ? formatFn(val)
    : val

  formatted = formatted === void 0 || formatted === null
    ? ''
    : String(formatted)

  formatted = formatted.split('"').join('""')
  return `"${formatted}"`
}


export default {
  setup () {
    const $q = useQuasar()
    return {
      columns,
      rows,
      exportTable () {
        const content = [columns.map(col => wrapCsvValue(col.label))].concat(
          rows.map(row => columns.map(col => wrapCsvValue(
            typeof col.field === 'function'
              ? col.field(row)
              : row[ col.field === void 0 ? col.name : col.field ],
            col.format
          )).join(','))
        ).join('\r\n')

        const status = exportFile(
          'table-export.csv',
          content,
          'text/csv'
        )

        if (status !== true) {
          $q.notify({
            message: 'Browser denied file download...',
            color: 'negative',
            icon: 'warning'
          })
        }
      }
    }
  }
}
</script>
