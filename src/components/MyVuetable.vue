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
        'name', 'email',
        {
          name: 'birthdate',
          titleClass: 'has-text-centered',
          dataClass: 'has-text-centered'
        },
        {
          name: 'nickname',
        },
        {
          name: 'gender',
          titleClass: 'has-text-centered',
          dataClass: 'has-text-centered'
        },
        {
          name: 'salary',
          titleClass: 'has-text-centered',
          dataClass: 'has-text-right'
        }
      ]
    }
  }
}
</script>
