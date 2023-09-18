<template>
  <div>
    <svg ref="boxPlot"></svg>
  </div>
</template>

<script>
import * as d3 from 'd3';

export default {
  mounted() {
    this.renderBoxPlot();
  },
  methods: {
    renderBoxPlot() {
      // Datos de ejemplo para el gráfico de caja
      const data = [
        [45, 50, 55, 60, 70, 75, 80],
        [35, 40, 45, 50, 55, 60, 65],
        [60, 65, 70, 75, 80, 85, 90],
      ];

      const width = 400;
      const height = 300;
      const margin = { top: 20, right: 30, bottom: 30, left: 40 };

      const svg = d3.select(this.$refs.boxPlot)
        .attr('width', width)
        .attr('height', height);

      const xScale = d3.scaleBand()
        .domain(["Grupo 1", "Grupo 2", "Grupo 3"])
        .range([margin.left, width - margin.right])
        .padding(0.4);

      const yScale = d3.scaleLinear()
        .domain([0, d3.max(data.flat())])
        .nice()
        .range([height - margin.bottom, margin.top]);

      svg.selectAll()
        .data(data)
        .enter().append('g')
        .attr('transform', (d, i) => `translate(${xScale(`Grupo ${i + 1}`)},0)`)
        .call(g => {
          g.append('line')
            .attr('y1', d => yScale(d[0]))
            .attr('y2', d => yScale(d[6]))
            .attr('x1', xScale.bandwidth() / 2)
            .attr('x2', xScale.bandwidth() / 2)
            .attr('stroke', 'black')
            .attr('stroke-width', 2);
          
          g.append('rect')
            .attr('x', xScale.bandwidth() * 0.25)
            .attr('y', d => yScale(d[4]))
            .attr('width', xScale.bandwidth() * 0.5)
            .attr('height', d => yScale(d[2]) - yScale(d[4]))
            .attr('fill', 'white')
            .attr('stroke', 'black')
            .attr('stroke-width', 2);
          
          g.selectAll('circle')
            .data(d => d)
            .enter().append('circle')
            .attr('cx', xScale.bandwidth() / 2)
            .attr('cy', d => yScale(d))
            .attr('r', 5)
            .attr('fill', 'black');
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
