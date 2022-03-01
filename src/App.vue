<template>
  <div id="app">
   <div>
        <Chart></Chart>
  </div>
  </div>
</template>

<script>
import Chart from './components/Chart.vue'
import Tooltip from './components/Tooltip.vue'

export default {
  name: 'App',
  
  components: {
      Tooltip,
      Chart
  },
  data() {
      return {
       
      }
  },
}

</script>

<style>



</style>
