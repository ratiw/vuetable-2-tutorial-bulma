<template>
  <nav v-if="tablePagination && tablePagination.last_page > 1" :class="['pagination', location]">
    <a @click="loadPage('prev')"
      :class="['pagination-previous', isOnFirstPage ? css.disabledClass : '']"
      v-html="prevText"
    ></a>
    <a @click="loadPage('next')"
      :class="['pagination-next', isOnLastPage ? css.disabledClass : '']"
      v-html="nextText"
    ></a>
    <ul :class="[css.listClass]">
      <template v-if="totalPagesLessThanWindowSize">
        <li v-for="n in totalPage">
          <a @click="loadPage(n)"
            :class="[css.linkClass, isCurrentPage(n) ? css.activeClass : '']"
            v-html="n"
          ></a>
        </li>
      </template>
      <template v-else>
        <li>
          <a @click="loadPage(1)"
            :class="[css.linkClass, isOnFirstPage ? css.disabledClass : '']"
          >1</a>
        </li>
        <li>
          <span :class="css.ellipsisClass">&hellip;</span>
        </li>
        <li v-for="n in windowSize-2">
          <a @click="loadPage(windowStart+n)"
            :class="[css.linkClass, isCurrentPage(windowStart+n) ? css.activeClass : '']"
            v-html="windowStart+n">
          </a>
        </li>
        <li>
          <span :class="css.ellipsisClass">&hellip;</span>
        </li>
        <li>
          <a @click="loadPage(totalPage)"
            :class="[css.linkClass, isOnLastPage ? css.disabledClass : '']"
            v-html="totalPage">
          </a>
        </li>
      </template>
    </ul>
  </nav>
</template>
<script>
import PaginationMixin from 'vuetable-2/src/components/VuetablePaginationMixin'

export default {
  mixins: [PaginationMixin],
  props: {
    location: {
      type: String,
      default: ''
    },
    prevText: {
      type: String,
      default: 'Previous'
    },
    nextText: {
      type: String,
      default: 'Next page'
    },
    css: {
      type: Object,
      default () {
        return {
          activeClass: 'is-current',
          disabledClass: 'is-disabled',
          listClass: 'pagination-list',
          linkClass: 'pagination-link',
          ellipsisClass: 'pagination-ellipsis'
        }
      }
    }
  },
  computed: {
    totalPagesLessThanWindowSize () {
      return this.totalPage < (this.onEachSide * 2) + 4
    }
  }
}
</script>
