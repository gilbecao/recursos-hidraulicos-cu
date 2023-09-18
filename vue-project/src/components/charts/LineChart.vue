<template>
  <div>
    <svg ref="lineChart"></svg>
  </div>
</template>

<script>
import * as d3 from 'd3';

export default {
  mounted() {
    this.renderLineChart();
  },
  methods: {
    renderLineChart() {
      // Datos de ejemplo para el gráfico de líneas
      const data = [
        { semana: 1, Turbidez: 10, Temperatura: 25, Conductividad: 350, PH: 7, "Suspensión de Sólidos Totales": 15 },
        { semana: 2, Turbidez: 9, Temperatura: 24, Conductividad: 345, PH: 7.1, "Suspensión de Sólidos Totales": 14 },
        { semana: 3, Turbidez: 8, Temperatura: 23, Conductividad: 340, PH: 7.2, "Suspensión de Sólidos Totales": 16 },
        // Agrega más datos aquí...
      ];

      const width = 400;
      const height = 300;
      const margin = { top: 20, right: 30, bottom: 30, left: 40 };

      const svg = d3.select(this.$refs.lineChart)
        .attr('width', width)
        .attr('height', height);

      const xScale = d3.scaleLinear()
        .domain([1, data.length])
        .range([margin.left, width - margin.right]);

      const yScale = d3.scaleLinear()
        .domain([0, d3.max(data, d => d3.max(Object.values(d).slice(1)))])
        .nice()
        .range([height - margin.bottom, margin.top]);

      const colorScale = d3.scaleOrdinal()
        .domain(Object.keys(data[0]).slice(1))
        .range(d3.schemeCategory10);

      const line = d3.line()
        .x(d => xScale(d.semana))
        .y(d => yScale(d.value))
        .curve(d3.curveBasis);

      Object.keys(data[0]).slice(1).forEach(variable => {
        svg.append('path')
          .datum(data)
          .attr('fill', 'none')
          .attr('stroke', colorScale(variable))
          .attr('stroke-width', 2)
          .attr('d', line.x(d => xScale(d.semana)).y(d => yScale(d[variable])));

        // Puntos de datos
        svg.selectAll('.point')
          .data(data)
          .enter().append('circle')
          .attr('cx', d => xScale(d.semana))
          .attr('cy', d => yScale(d[variable]))
          .attr('r', 4)
          .attr('fill', colorScale(variable));
      });

      // Eje X
      svg.append('g')
        .attr('transform', `translate(0,${height - margin.bottom})`)
        .call(d3.axisBottom(xScale));

      // Eje Y
      svg.append('g')
        .attr('transform', `translate(${margin.left},0)`)
        .call(d3.axisLeft(yScale));
    },
  },
};
</script>

<style scoped>
/* Estilos personalizados para tu componente, si es necesario */
</style>
