<template>
  <vuetable ref="vuetable"
    api-url="http://vuetable.ratiw.net/api/users"
    :fields="fields"
    :css="css"
  ></vuetable>
</template>

<script>
import Vuetable from 'vuetable-2/src/components/Vuetable'

export default {
  components: {
    Vuetable
  },
  data () {
    return {
      css: {
        tableClass: 'table is-bordered is-striped'
      },
      fields: [
        'name', 'email', 'birthdate',
        'address.line1', 'address.line2', 'address.zipcode'
      ]
    }
  }
}
</script>
