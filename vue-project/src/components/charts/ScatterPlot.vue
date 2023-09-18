<template>
  <div>
    <svg ref="scatterPlot"></svg>
  </div>
</template>

<script>
import * as d3 from 'd3';

export default {
  mounted() {
    this.renderScatterPlot();
  },
  methods: {
    renderScatterPlot() {
      // Datos de ejemplo para el gráfico de dispersión
      const data = [
        { semana: 1, Turbidez: 10 },
        { semana: 2, Turbidez: 9 },
        { semana: 3, Turbidez: 8 },
        // Agrega más datos aquí...
      ];

      const width = 400;
      const height = 300;
      const margin = { top: 20, right: 30, bottom: 30, left: 40 };

      const svg = d3.select(this.$refs.scatterPlot)
        .attr('width', width)
        .attr('height', height);

      const xScale = d3.scaleLinear()
        .domain([1, d3.max(data, d => d.semana)])
        .nice()
        .range([margin.left, width - margin.right]);

      const yScale = d3.scaleLinear()
        .domain([0, d3.max(data, d => d.Turbidez)])
        .nice()
        .range([height - margin.bottom, margin.top]);

      svg.selectAll('circle')
        .data(data)
        .enter().append('circle')
        .attr('cx', d => xScale(d.semana))
        .attr('cy', d => yScale(d.Turbidez))
        .attr('r', 4)
        .attr('fill', 'steelblue');

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
