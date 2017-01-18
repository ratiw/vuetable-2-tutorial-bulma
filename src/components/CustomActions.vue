<template>
  <div class="custom-actions">
    <a class="button is-outlined" @click="itemAction('view-item', rowData, rowIndex)"><i class="fa fa-search"></i></a>
    <a class="button is-outlined" @click="itemAction('edit-item', rowData, rowIndex)"><i class="fa fa-pencil"></i></a>
    <a class="button is-outlined" @click="itemAction('delete-item', rowData, rowIndex)"><i class="fa fa-trash"></i></a>
  </div>
</template>

<script>
export default {
  props: {
    rowData: {
      type: Object,
      required: true
    },
    rowIndex: {
      type: Number
    }
  },
  methods: {
    itemAction (action, data, index) {
      console.log('custom-actions: ' + action, data.name, index)
    }
  }
}
</script>

<style>
  .custom-actions button.ui.button {
    padding: 8px 8px;
  }
  .custom-actions button.ui.button > i.icon {
    margin: auto !important;
  }
</style>
