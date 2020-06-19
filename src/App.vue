<template>
  <div id="app">
    <item-selection></item-selection>
  </div>
</template>

<script>
import ItemSelection from './components/ItemSelection.vue'

export default {
  name: 'app',
  data(){
    return {
    }
  },
  components: {
    ItemSelection
  }
}
</script>

<style>
body {
  margin: 0;
}
</style>
