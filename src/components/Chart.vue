<template>
  <!-- Allocate chart inside div container -->
  <div>{{createChart()}}</div>
</template>

<script>
export default {
  props: ['points'],
  methods: {
    // Define method which creates a chart.
    createChart() {
      // Import lcjs library
        const lcjs = require('@arction/lcjs')
        // Create chartXY
        const chart = lcjs.lightningChart().ChartXY()
        // Set chart title
        chart.setTitle('Getting Started')
        // Add line series to the chart
        const lineSeries = chart.addLineSeries()
        // Set stroke style of the line
        lineSeries.setStrokeStyle((style) => style.setThickness(5))
        // Add data point to the line series
        lineSeries.add(this.points)
    }
  }
}

</script>


