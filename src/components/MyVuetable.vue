<template>
  <div class="container">
    <nav class="level is-marginless">
      <div class="level-left"></div>
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

export default {
  components: {
    Vuetable,
    BulmaPagination,
    VuetablePaginationInfo
  },
  data () {
    return {
      css: {
        tableClass: 'table is-bordered is-striped'
      },
      fields: [
        'name', 'email',
        {
          name: 'birthdate',
          titleClass: 'has-text-centered',
          dataClass: 'has-text-centered',
          callback: 'formatDate|DD-MM-YYYY'
        },
        {
          name: 'nickname',
          callback: 'allcap'
        },
        {
          name: 'gender',
          titleClass: 'has-text-centered',
          dataClass: 'has-text-centered',
          callback: 'genderLabel'
        },
        {
          name: 'salary',
          titleClass: 'has-text-centered',
          dataClass: 'has-text-right',
          callback: 'formatNumber'
        }
      ]
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
    }
  }
}
</script>
