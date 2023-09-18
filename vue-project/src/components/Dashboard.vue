<template>
  <div>
    <div class="dashboard-item">
      <h2>Area Chart (Stacked)</h2>
      <AreaChartStacked />
    </div>
    <div class="dashboard-item">
      <h2>Bar Chart (Grouped)</h2>
      <BarChartGrouped />
    </div>
    <div class="dashboard-item">
      <h2>Bar Chart (Stacked)</h2>
      <BarChartStacked />
    </div>
    <div class="dashboard-item">
      <h2>Box Plot</h2>
      <BoxPlot />
    </div>
    <div class="dashboard-item">
      <h2>Candlestick Chart</h2>
      <CandlestickChart />
    </div>
    <div class="dashboard-item">
      <h2>Heatmap</h2>
      <Heatmap />
    </div>
    <div class="dashboard-item">
      <h2>Line Chart</h2>
      <LineChart />
    </div>
    <div class="dashboard-item">
      <h2>Pie Chart</h2>
      <PieChart />
    </div>
    <div class="dashboard-item">
      <h2>Radar Chart</h2>
      <RadarChart />
    </div>
    <div class="dashboard-item">
      <h2>Scatter Plot</h2>
      <ScatterPlot />
    </div>
  </div>
</template>

<script>
import AreaChartStacked from './charts/AreaChartStacked.vue';
import BarChartGrouped from './charts/BarChartGrouped.vue';
import BarChartStacked from './charts/BarChartStacked.vue';
import BoxPlot from './charts/BoxPlot.vue';
import CandlestickChart from './charts/CandlestickChart.vue'
import HeatMap from './charts/HeatMap.vue';
import LineChart from './charts/LineChart.vue';
import PieChart from './charts/PieChart.vue';
import RadarChart from './charts/RadarChart.vue';
import ScatterPlot from './charts/ScatterPlot.vue';


export default {
  components: {
    AreaChartStacked,
    BarChartGrouped,
    BarChartStacked,
    BoxPlot,
    CandlestickChart,
    HeatMap,
    LineChart,
    PieChart,
    RadarChart,
    ScatterPlot,
  },
};
</script>

<style scoped>
.dashboard-item {
  margin: 20px;
  padding: 20px;
  border: 1px solid #ccc;
  background-color: #f5f5f5;
}
</style>
