<template>
  <div id="app">
    <!-- Create Chart component and pass data points to the created chart -->
    <Chart :points="points"/>
  </div>
</template>

<script>
import Chart from './components/Chart.vue'

export default {
  name: 'app',
  data: () => {
    return {
      // Data points for the chart
      points: [
        { x: 0, y: 0 },
        { x: 1, y: 7 },
        { x: 2, y: 3 },
        { x: 3, y: 10 }
      ]
    }
  },
  components: {
    // Include Chart component to the app
    Chart
  }
}
</script>
