<template lang="pug">
  component-view(v-bind:doc="doc")
</template>

<script>
  export default {
    data () {
      return {
        doc: {
          component: 'tables',
          edit: 'DatatablesView',
          title: 'Data tables',
          id: '#data-tables-view',
          desc: `The <code>v-data-table</code> component is used for displaying tabular data. Features include sorting, searching, pagination, inline-editing, header tooltips, and row selection.`,
          examples: [
            { header: 'Standard', file: 'tables/1', desc: 'The standard data-table contains data with no additional functionality. You can opt out of displaying table actions that allow you to control the pagination of information with the <code>hide-actions</code> prop.' },
            { header: 'Selectable rows', file: 'tables/2', desc: 'Selectable rows allow you to perform an action on specific and all rows.' },
            { header: 'Search with custom page text', file: 'tables/3', desc: 'The data table exposes a <code>search</code> prop that allows you to filter your data.' },
            { header: 'External pagination', file: 'tables/4', desc: 'Pagination can be controlled externally by using the <code>pagination</code> prop. Remember that you must apply the <code>.sync</code> modifier.' },
            { header: 'External sorting', file: 'tables/5', desc: 'Sorting can also be controlled externally by using the <code>pagination</code> prop. Remember that you must apply the <code>.sync</code> modifier. You can also use the prop to set the default sorted column.' },
            { header: 'Paginate and sort server-side', file: 'tables/6', desc: 'If you\'re loading data from a backend and want to paginate and sort the results before displaying them, you can use the <code>total-items</code> prop. Defining this prop will disable the built-in sorting and pagination, and you will instead need to use the <code>pagination</code> prop to listen for changes. Use the <code>loading</code> prop to display a progress bar while fetching data.'}
          ],
          props: {
            'v-data-table': {
              params: [
                [
                  'headers',
                  'Array',
                  '[]',
                  'The array of headers'
                ],
                [
                  'header-text',
                  'String',
                  'text',
                  'If using an object, the text value for the header'
                ],
                [
                  'hide-actions',
                  'Boolean',
                  'False',
                  'Hide the table actions'
                ],
                [
                  'items',
                  'Array',
                  '[]',
                  'The array of table rows'
                ],
                [
                  'no-data-text',
                  'String',
                  'No data available in table',
                  'Display text when there is no table data.'
                ],
                [
                  'no-results-text',
                  'String',
                  'No matching records found',
                  'Display text when there are no filtered results.'
                ],
                [
                  'rows-per-page-text',
                  'String',
                  'Rows per page:',
                  'The default rows per page text'
                ],
                [
                  'rows-per-page-items',
                  'Array',
                  '[5, 15, 25, { text: "All", value: -1 }]',
                  'The default values for the rows-per-page dropdown'
                ],
                [
                  'select-all',
                  '[Boolean, String]',
                  'False',
                  'Adds header row select all checkbox. Can either be a String which specifies which color is applied to the button (primary, secondary, success, info, warning, error) or a Boolean (which uses the primary color).'
                ],
                [
                  'selected-key',
                  'String',
                  'id',
                  'Determines the item value used for identifying selected items.'
                ],
                [
                  'search',
                  'String',
                  '-',
                  'The search model for filtering results'
                ],
                [
                  'filter',
                  'Function',
                  `(val, search) => {
                  defined', 'boolean'].indexOf(typeof val) === -1 &&
                      val.toString().toLowerCase().indexOf(search) !== -1
                  }`,
                  'The filtering method for search'
                ],
                [
                  'custom-filter',
                  'Function',
                  '-',
                  'Custom search filter'
                ],
                [
                  'custom-sort',
                  'Function',
                  '-',
                  'Custom sort filter'
                ],
                [
                  'total-items',
                  'Number',
                  '-',
                  'Manually sets total number of row items, which disables built-in sort and pagination. Used together with pagination prop to enable server-side sort and pagination.'
                ],
                [
                  'loading',
                  '[Boolean, String]',
                  'False',
                  'Displays progress bar. Can either be a String which specifies which color is applied to the progress bar (primary, secondary, success, info, warning, error) or a Boolean (which uses the primary color)'
                ],
                [
                  'pagination.sync',
                  'Object',
                  `{
                    page: 1,
                    rowsPerPage: 5,
                    descending: false,
                    totalItems: 0
                  }`,
                  'Used to control pagination and sorting from outside the data table. Can also be used to set default sorted column.'
                ]
              ],
              model: {
                type: ['Array'],
                default: '',
                description: 'Holds selected row items'
              }
            },
            'v-edit-dialog': {
              params: [
                [
                  'cancel-text',
                  'String',
                  'Cancel',
                  `Set's the default text for the cancel button when using the <code>large</code> prop`
                ],
                [
                  'save-text',
                  'String',
                  'Save',
                  `Set's the default text for the save button when using the <code>large</code> prop`
                ],
                [
                  'Large',
                  'Boolean',
                  'False',
                  'Attachs a submit and cancel button to the dialog'
                ],
                [
                  'Lazy',
                  'Boolean',
                  'False',
                  'Lazily load the dialog contents'
                ],
                [
                  'transition',
                  'String',
                  'v-slide-x-reverse-transition',
                  'The transition of the edit dialog'
                ]
              ]
            }
          },
          slots: {
            'v-data-table': {
              params: [
                [
                  'scope[headers]',
                  'The scoped slot for modifying headers.'
                ],
                [
                  'scope[items]',
                  'The scoped slot for templating the row display. Available props are the currently iterated <code>item</code> and its <code>index</code>.'
                ],
                [
                  'scope[pageText]',
                  'The scoped slot for adding custom page text.'
                ]
              ]
            }
          },
          events: {
            'v-data-table': {
              params: [
                ['input', 'Array', 'Array will contain selected rows'],
              ]
            },
            'v-edit-dialog': {
              params: [
                ['open', '-', 'Edit dialog opened'],
                ['close', '-', 'Edit dialog closed'],
                ['cancel', '-', 'Cancel button was clicked'],
                ['save', '-', 'Save button was clicked'],
              ]
            },
          }
        }
      }
    },

    mounted () {
      this.$emit('view', this.meta())
    },

    preFetch () {
      return this.methods.meta()
    },

    methods: {
      saving () {
        console.log('I saved!')
      },
      meta () {
        return {
          title: 'Data tables | Vuetify.js',
          h1: 'Data tables',
          description: 'The v-data-table component is used for displaying tabular data. Features include sorting, searching, pagination, inline-editing, header tooltips, and row selection.',
          keywords: 'vuetify, components, data tables'
        }
      }
    }
  }
</script>

<style lang="stylus">
  #data-tables-view
    max-width: 1200px
</style>
