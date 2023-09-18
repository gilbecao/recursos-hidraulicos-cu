<template>
  <div>
    <svg ref="pieChart"></svg>
  </div>
</template>

<script>
import * as d3 from 'd3';

export default {
  mounted() {
    this.renderPieChart();
  },
  methods: {
    renderPieChart() {
      // Datos de ejemplo para el gráfico de pastel
      const data = [
        { variable: "Turbidez", valor: 10 },
        { variable: "Temperatura", valor: 25 },
        { variable: "Conductividad", valor: 350 },
        { variable: "PH", valor: 7 },
        { variable: "Suspensión de Sólidos Totales", valor: 15 },
      ];

      const width = 400;
      const height = 300;

      const svg = d3.select(this.$refs.pieChart)
        .attr('width', width)
        .attr('height', height);

      const radius = Math.min(width, height) / 2;

      const pie = d3.pie()
        .value(d => d.valor);

      const arc = d3.arc()
        .innerRadius(0)
        .outerRadius(radius);

      const colorScale = d3.scaleOrdinal()
        .domain(data.map(d => d.variable))
        .range(d3.schemeCategory10);

      const pieData = pie(data);

      const g = svg.append('g')
        .attr('transform', `translate(${width / 2},${height / 2})`);

      g.selectAll('path')
        .data(pieData)
        .enter().append('path')
        .attr('d', arc)
        .attr('fill', d => colorScale(d.data.variable));

      // Leyenda
      const legend = svg.selectAll('.legend')
        .data(pieData)
        .enter().append('g')
        .attr('class', 'legend')
        .attr('transform', (d, i) => `translate(0,${i * 20})`);

      legend.append('rect')
        .attr('width', 18)
        .attr('height', 18)
        .attr('fill', d => colorScale(d.data.variable));

      legend.append('text')
        .attr('x', 24)
        .attr('y', 9)
        .attr('dy', '.35em')
        .text(d => d.data.variable);
    },
  },
};
</script>

<style scoped>
/* Estilos personalizados para tu componente, si es necesario */
</style>
