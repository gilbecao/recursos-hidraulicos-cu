<template>
  <div>
    <svg ref="stackedAreaChart"></svg>
  </div>
</template>

<script>
import * as d3 from 'd3';

export default {
  mounted() {
    this.renderStackedAreaChart();
  },
  methods: {
    renderStackedAreaChart() {
      // Datos de ejemplo para el gráfico de área apilada
      const data = [
        {
          fecha: "2023-09-01",
          Turbidez: 10.5,
          Temperatura: 25.3,
          Conductividad: 350.2,
          PH: 7.2,
          "Suspensión de Sólidos Totales": 15.8
        },
        {
          fecha: "2023-09-02",
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

      const svg = d3.select(this.$refs.stackedAreaChart)
        .attr('width', width)
        .attr('height', height);

      const parseDate = d3.timeParse("%Y-%m-%d");

      // Convertir las fechas de cadena a objetos Date
      data.forEach(d => {
        d.fecha = parseDate(d.fecha);
      });

      // Crear una escala de tiempo para el eje X
      const xScale = d3.scaleTime()
        .domain(d3.extent(data, d => d.fecha))
        .range([margin.left, width - margin.right]);

      // Crear una escala lineal para el eje Y
      const yScale = d3.scaleLinear()
        .domain([0, d3.max(data, d => d3.sum(Object.values(d).slice(1)))])
        .nice()
        .range([height - margin.bottom, margin.top]);

      const colorScale = d3.scaleOrdinal(d3.schemeCategory10);

      const stack = d3.stack()
        .keys(Object.keys(data[0]).slice(1))
        .order(d3.stackOrderNone)
        .offset(d3.stackOffsetNone);

      const series = stack(data);

      const area = d3.area()
        .x(d => xScale(d.data.fecha))
        .y0(d => yScale(d[0]))
        .y1(d => yScale(d[1]))
        .curve(d3.curveBasis);

      svg.selectAll('.area')
        .data(series)
        .enter().append('path')
        .attr('class', 'area')
        .attr('d', area)
        .style('fill', (d, i) => colorScale(i));

      // Eje X
      svg.append('g')
        .attr('transform', `translate(0,${height - margin.bottom})`)
        .call(d3.axisBottom(xScale).ticks(5));

      // Eje Y
      svg.append('g')
        .attr('transform', `translate(${margin.left},0)`)
        .call(d3.axisLeft(yScale));
    },
  },
};
</script>
