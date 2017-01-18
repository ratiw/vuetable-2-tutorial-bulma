<template>
  <div class="container">
    <nav class="level is-marginless">
      <div class="level-left">
        <div class="level-item">
          <filter-bar></filter-bar>
        </div>
      </div>
      <div class="level-right">
        <vuetable-pagination-info ref="paginationInfo"
        ></vuetable-pagination-info>
      </div>
    </nav>
    <vuetable ref="vuetable"
      api-url="http://vuetable.ratiw.net/api/users"
      :fields="fields"
      :css="css"
      pagination-path=""
      :multi-sort="true"
      multi-sort-key="ctrl"
      :sort-order="sortOrder"
      detail-row-component="my-detail-row"
      :append-params="moreParams"
      @vuetable:cell-clicked="onCellClicked"
      @vuetable:pagination-data="onPaginationData"
    ></vuetable>
    <bulma-pagination ref="pagination"
      @vuetable-pagination:change-page="onChangePage"
    ></bulma-pagination>
  </div>
</template>

<script>
import accounting from 'accounting'
import moment from 'moment'
import Vuetable from 'vuetable-2/src/components/Vuetable'
import BulmaPagination from './BulmaPagination'
import VuetablePaginationInfo from 'vuetable-2/src/components/VuetablePaginationInfo'
import Vue from 'vue'
import VueEvents from 'vue-events'
Vue.use(VueEvents)
import CustomActions from './CustomActions'
import DetailRow from './DetailRow'
import FilterBar from './FilterBar'

Vue.component('custom-actions', CustomActions)
Vue.component('my-detail-row', DetailRow)

export default {
  components: {
    Vuetable,
    BulmaPagination,
    VuetablePaginationInfo,
    FilterBar
  },
  data () {
    return {
      css: {
        tableClass: 'table is-bordered is-striped',
        ascendingIcon: 'fa fa-chevron-up',
        descendingIcon: 'fa fa-chevron-down',
        sortHandleIcon: 'fa fa-bars',
      },
      fields: [
        {
          name: '__checkbox',
          title: '#',
          titleClass: 'has-text-centered',
          dataClass: 'has-text-centered'
        },
        {
          name: 'name',
          sortField: 'name'
        },
        {
          name: 'email',
          sortField: 'email',
        },
        {
          name: 'age',
          sortField: 'birthdate',
          titleClass: 'has-text-centered',
          dataClass: 'has-text-centered'
        },
        {
          name: 'birthdate',
          sortField: 'birthdate',
          titleClass: 'has-text-centered',
          dataClass: 'has-text-centered',
          callback: 'formatDate|DD-MM-YYYY'
        },
        {
          name: 'nickname',
          sortField: 'nickname',
          callback: 'allcap'
        },
        {
          name: 'gender',
          sortField: 'gender',
          titleClass: 'has-text-centered',
          dataClass: 'has-text-centered',
          callback: 'genderLabel'
        },
        {
          name: 'salary',
          sortField: 'salary',
          titleClass: 'has-text-centered',
          dataClass: 'has-text-right',
          callback: 'formatNumber'
        },
        {
          name: '__component:custom-actions',
          title: 'Actions',
          titleClass: 'has-text-centered',
          dataClass: 'has-text-centered',
        }
      ],
      sortOrder: [
          {
            field: 'email',
            sortField: 'email',
            direction: 'asc'
          }
      ],
      moreParams: {}
    }
  },
  methods: {
    allcap (value) {
      return value.toUpperCase()
    },
    genderLabel (value) {
      return value === 'M'
        ? '<span class="tag is-primary is-medium"><span class="icon"><i class="fa fa-mars"></i></span>&nbsp;Male</span>'
        : '<span class="tag is-danger is-medium"><span class="icon"><i class="fa fa-venus"></i></span>&nbsp;Female</span>'
    },
    formatNumber (value) {
      return accounting.formatNumber(value, 2)
    },
    formatDate (value, fmt = 'D MMM YYYY') {
      return (value == null)
        ? ''
        : moment(value, 'YYYY-MM-DD').format(fmt)
    },
    onPaginationData (paginationData) {
      this.$refs.pagination.setPaginationData(paginationData)
      this.$refs.paginationInfo.setPaginationData(paginationData)
    },
    onChangePage (page) {
      this.$refs.vuetable.changePage(page)
    },
    onCellClicked (data, field, event) {
        console.log('cellClicked: ', field.name)
        this.$refs.vuetable.toggleDetailRow(data.id)
    }
  },
  events: {
    'filter-set' (filterText) {
      this.moreParams = {
        'filter': filterText
      }
      Vue.nextTick( () => this.$refs.vuetable.refresh())
    },
    'filter-reset' () {
      this.moreParams = {}
      this.$refs.vuetable.refresh()
      Vue.nextTick( () => this.$refs.vuetable.refresh())
    }
  }
}
</script>
