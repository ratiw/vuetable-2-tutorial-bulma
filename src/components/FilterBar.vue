<template>
  <div class="filter-bar control is-grouped">
    <p class="control is-expanded has-addons">
      <input id="search" type="text" v-model="filterText" class="input" @keyup.enter="doFilter" placeholder="Search name, nickname, or email">
      <a @click="doFilter" class="button is-primary">Go</a>
    </p>
    <p class="control">
      <a @click="resetFilter" class="button">Reset</a>
    </p>
  </div>
</template>

<script>
export default {
  data () {
    return {
      filterText: ''
    }
  },
  methods: {
    doFilter () {
      this.$events.fire('filter-set', this.filterText)
    },
    resetFilter () {
      this.filterText = ''  // clear the text in text input
      this.$events.fire('filter-reset')
    }
  }
}
</script>
<style>
#search {
  width: 300px;
}
</style>
