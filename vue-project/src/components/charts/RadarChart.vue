<template>
  <div>
    <svg ref="radarChart"></svg>
  </div>
</template>

<script>
import * as d3 from 'd3';

export default {
  mounted() {
    this.renderRadarChart();
  },
  methods: {
    renderRadarChart() {
      // Datos de ejemplo para el gráfico radar
      const data = [
        { variable: "Turbidez", valor: 10 },
        { variable: "Temperatura", valor: 25 },
        { variable: "Conductividad", valor: 350 },
        { variable: "PH", valor: 7 },
        { variable: "Suspensión de Sólidos Totales", valor: 15 },
      ];

      const width = 400;
      const height = 300;

      const svg = d3.select(this.$refs.radarChart)
        .attr('width', width)
        .attr('height', height);

      const centerX = width / 2;
      const centerY = height / 2;
      const radius = Math.min(centerX, centerY) * 0.8;

      const maxValue = d3.max(data, d => d.valor);

      const angleSlice = (Math.PI * 2) / data.length;

      const radarLine = d3.lineRadial()
        .radius(d => (d.valor / maxValue) * radius)
        .angle((d, i) => i * angleSlice);

      const colorScale = d3.scaleOrdinal()
        .domain(data.map(d => d.variable))
        .range(d3.schemeCategory10);

      svg.append('g')
        .attr('class', 'radar-chart')
        .attr('transform', `translate(${centerX},${centerY})`)
        .selectAll('.radar-path')
        .data([data])
        .enter()
        .append('path')
        .attr('class', 'radar-path')
        .attr('d', radarLine)
        .attr('fill', 'none')
        .attr('stroke', colorScale(data[0].variable));

      // Eje radial
      const axisGrid = svg.append('g')
        .attr('class', 'axis-grid');

      data.forEach((d, i) => {
        const angle = i * angleSlice;
        axisGrid.append('line')
          .attr('x1', centerX)
          .attr('y1', centerY)
          .attr('x2', centerX + Math.cos(angle) * radius)
          .attr('y2', centerY + Math.sin(angle) * radius);
      });

      // Etiquetas de las variables
      const axisLabels = svg.append('g')
        .attr('class', 'axis-labels');

      data.forEach((d, i) => {
        const angle = i * angleSlice;
        axisLabels.append('text')
          .attr('x', centerX + Math.cos(angle) * (radius + 10))
          .attr('y', centerY + Math.sin(angle) * (radius + 10))
          .text(d.variable);
      });
    },
  },
};
</script>

<style scoped>
/* Estilos personalizados para tu componente, si es necesario */
</style>
