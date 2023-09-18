<template>
  <div>
    <svg ref="candlestickChart"></svg>
  </div>
</template>

<script>
import * as d3 from 'd3';

export default {
  mounted() {
    this.renderCandlestickChart();
  },
  methods: {
    renderCandlestickChart() {
      // Datos de ejemplo para el gráfico de velas
      const data = [
        { fecha: "2023-09-01", apertura: 100, cierre: 120, maximo: 130, minimo: 90 },
        { fecha: "2023-09-02", apertura: 120, cierre: 110, maximo: 125, minimo: 105 },
        { fecha: "2023-09-03", apertura: 110, cierre: 105, maximo: 115, minimo: 100 },
        // Agrega más datos aquí...
      ];

      const width = 400;
      const height = 300;
      const margin = { top: 20, right: 30, bottom: 30, left: 40 };

      const svg = d3.select(this.$refs.candlestickChart)
        .attr('width', width)
        .attr('height', height);

      const xScale = d3.scaleBand()
        .domain(data.map(d => d.fecha))
        .range([margin.left, width - margin.right])
        .padding(0.1);

      const yScale = d3.scaleLinear()
        .domain([d3.min(data, d => d.minimo), d3.max(data, d => d.maximo)])
        .nice()
        .range([height - margin.bottom, margin.top]);

      svg.selectAll()
        .data(data)
        .enter().append('g')
        .attr('transform', d => `translate(${xScale(d.fecha) + xScale.bandwidth() / 2},0)`)
        .each(function (d) {
          d3.select(this).append('line')
            .attr('y1', yScale(d.minimo))
            .attr('y2', yScale(d.maximo))
            .attr('stroke', 'black')
            .attr('stroke-width', 2);

          const rect = d.cierre > d.apertura ?
            d3.select(this).append('rect')
              .attr('x', -10)
              .attr('y', yScale(d.cierre))
              .attr('width', 20)
              .attr('height', yScale(d.apertura) - yScale(d.cierre))
              .attr('fill', 'green') :
            d3.select(this).append('rect')
              .attr('x', -10)
              .attr('y', yScale(d.apertura))
              .attr('width', 20)
              .attr('height', yScale(d.cierre) - yScale(d.apertura))
              .attr('fill', 'red');
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
