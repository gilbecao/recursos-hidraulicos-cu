<template>
  <div>
    <svg ref="groupedBarChart"></svg>
  </div>
</template>

<script>
import * as d3 from 'd3';

export default {
  mounted() {
    this.renderGroupedBarChart();
  },
  methods: {
    renderGroupedBarChart() {
      // Datos de ejemplo para el gráfico de barras agrupadas
      const data = [
        {
          grupo: "Grupo 1",
          Turbidez: 10.5,
          Temperatura: 25.3,
          Conductividad: 350.2,
          PH: 7.2,
          "Suspensión de Sólidos Totales": 15.8
        },
        {
          grupo: "Grupo 2",
          Turbidez: 9.8,
          Temperatura: 24.5,
          Conductividad: 345.1,
          PH: 7.0,
          "Suspensión de Sólidos Totales": 16.3
        },
        // Agrega más datos aquí...
      ];

      const width = 400;
      const height = 300;
      const margin = { top: 20, right: 30, bottom: 30, left: 40 };

      const svg = d3.select(this.$refs.groupedBarChart)
        .attr('width', width)
        .attr('height', height);

      // Lista de variables en los datos
      const variables = Object.keys(data[0]).slice(1);

      const xScale = d3.scaleBand()
        .domain(data.map(d => d.grupo))
        .range([margin.left, width - margin.right])
        .padding(0.1);

      const yScale = d3.scaleLinear()
        .domain([0, d3.max(data, d => d3.max(variables, v => d[v]))])
        .nice()
        .range([height - margin.bottom, margin.top]);

      const colorScale = d3.scaleOrdinal()
        .domain(variables)
        .range(d3.schemeCategory10);

      svg.selectAll()
        .data(data)
        .enter().append('g')
        .attr('transform', d => `translate(${xScale(d.grupo)},0)`)
        .selectAll('.bar')
        .data(d => variables.map(v => ({ variable: v, value: d[v] })))
        .enter().append('rect')
        .attr('class', 'bar')
        .attr('x', d => xScale.bandwidth() / variables.length * variables.indexOf(d.variable))
        .attr('y', d => yScale(d.value))
        .attr('width', xScale.bandwidth() / variables.length)
        .attr('height', d => yScale(0) - yScale(d.value))
        .attr('fill', d => colorScale(d.variable));

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
