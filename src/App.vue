<template>
 <div id="app" style="height: 100vh">
  <ChartData :points="points"/>
 </div>
</template>

<script>
import ChartData from './components/Chart.vue'

export default {
  name: 'app',
  data: () => {
    return {
      // Data points for the chart
      points: [
        { x: 0, y: 0 },
        { x: 1, y: 7 },
        { x: 2, y: 3 },
        { x: 3, y: 10 },
        { x: 4, y:  12},
        { x: 5, y: 9 },
        { x: 6, y: 15 }
      ]
    }
    },

  components: {
    ChartData
  }
}
</script>
