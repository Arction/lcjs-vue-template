<template>
  <!-- Allocate chart inside div container -->
  <div :id="id" class="fill"></div>
</template>

<script>
import {lightningChart} from '@arction/lcjs'

export default {
  name: 'Chart',
  props: ['points'],
  data() {
    return {
      id: null,
      chart: null,
      lineSeries: null
    }
  },
  methods: {
    createChart() {
      // Create chartXY
      this.chart = lightningChart().ChartXY({containerId: `${this.id}`})
      // Set chart title
      this.chart.setTitle('Getting Started')
      // Add line series to the chart
      this.lineSeries = this.chart.addLineSeries()
      // Set stroke style of the line
      lineSeries.setStrokeStyle((style) => style.setThickness(5))
      // Add data points to the line series
      lineSeries.add(this.points)
    }
  },
  beforeMount() {
    // Generate random ID to us as the containerId for the chart and the target div id
    this.id = Math.trunc(Math.random() * 1000000)
  },
  mounted() {
    // Chart can only be created when the component has mounted the DOM because 
    // the chart needs the element with specified containerId to exist in the DOM
    this.createChart()
  },
  beforeDestroy() {
    // "dispose" should be called when the component is unmounted to free all the resources used by the chart
    this.lineSeries.dispose()
  }
}
</script>

<style scoped>
  .fill {
    height: 100%;
  }
</style>