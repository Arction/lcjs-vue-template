<template>
  <!-- Allocate chart inside div container -->
  <div :id="chartId" class="fill"></div>
</template>

<script>
import {lightningChart ,AxisScrollStrategies } from '@arction/lcjs'
import { createProgressiveTraceGenerator } from '@arction/xydata'

export default {
  name: 'Chart',
   // props: ['points'],
  data() {
    // Add the chart to the data in a way that Vue will not attach it's observers to it.
    // If the chart variable would be added in the return object, Vue would attach the observers and 
    // every time LightningChart JS made a change to any of it's internal variables, vue would try to observe the change and update.
    // Observing would slow down the chart a lot.
    this.chart = null
    return {
      chartId: null,
    }
  },
  methods: {
    createChart() {
      // Create chartXY
      this.chart = lightningChart().ChartXY({container: `${this.chartId}`})
      // Set chart title
    .setTitle('Axis Y Fitting to visible data')
  .setMouseInteractions(false)


    const lineSeries = this.chart.addLineSeries({
      dataPattern: {
        pattern: 'ProgressiveX',
      }
    })

    // Setup scrolling X Axis.
    const dataPointsHistory = 500
     this.chart.getDefaultAxisX()
      .setScrollStrategy(AxisScrollStrategies.progressive)
      .setInterval(0, dataPointsHistory)
      .setMouseInteractions(false)

    const axisY = this.chart.getDefaultAxisY()
      .setMouseInteractions(false)

    // Keep track of n last data points (visible data points).
    const lastYValues = []
    // Value that controls how often visible Y interval should be updated. Frequent updates can be CPU intensive especially if there are a lot of data points in view.
    const updateYViewIntervalMs = 100
    let lastYViewIntervalUpdate = 0

    createProgressiveTraceGenerator()
      .setNumberOfPoints(10000)
      .generate()
      .setStreamInterval(1000 / 60)
      .setStreamBatchSize(1)
      .setStreamRepeat(true)
      .toStream()
      .forEach(point => {
        // Add point to line series.
        lineSeries.add(point)

        // Keep track of n last data points (visible last points)
        if (lastYValues.length >= dataPointsHistory) {
          lastYValues.shift()
        }
        lastYValues.push(point.y)

        const tNow = window.performance.now()
        if (tNow - lastYViewIntervalUpdate >= updateYViewIntervalMs) {
          lastYViewIntervalUpdate = tNow
          // Calculate Y values range of visible data points.
          let yMin = Number.MAX_SAFE_INTEGER
          let yMax = -Number.MAX_SAFE_INTEGER
          for (const y of lastYValues) {
            yMin = Math.min(yMin, y)
            yMax = Math.max(yMax, y)
          }
          // Actively set displayed Y axis interval to the range of visible data points.
          axisY.setInterval(yMin, yMax, false, true)
        }
      })
  }
  },
  beforeMount() {
    // Generate random ID to us as the containerId for the chart and the target div id
    this.chartId = Math.trunc(Math.random() * 1000000)
  },
  mounted() {
    // Chart can only be created when the component has mounted the DOM because 
    // the chart needs the element with specified containerId to exist in the DOM
    this.createChart()
  },
  beforeDestroy() {
    // "dispose" should be called when the component is unmounted to free all the resources used by the chart
    this.chart.dispose()
  }
}
</script>

<style scoped>
  .fill {
    height: 100%;
  }
</style>
