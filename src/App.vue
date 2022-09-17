<template>
  <q-layout view="lHh Lpr lFf">
    <q-page-container>
      <Main />
    </q-page-container>
  </q-layout>
</template>

<script>
import { ref } from 'vue'
import Main from './components/Main.vue'

export default {
  name: 'LayoutDefault',

  components: {
    Main
  },

  setup () {
    return {
      leftDrawerOpen: ref(false)
    }
  }
}
</script>
