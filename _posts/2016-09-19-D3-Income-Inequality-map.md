---
layout: post
title:  "D3 Experiments: Map of Income Inequality by County"
date:   2016-09-19
categories: visualization
tags: javascript,D3,visualization,map,gini
---

<style>
.counties {
  fill: none;
  stroke: #c2c2c2;
  stroke-width: .5px;
  stroke-linejoin: miter;
  stroke-linecap: miter;
}

.state-borders {
  fill: none;
  stroke: #fff;
  stroke-linejoin: round;
  stroke-linecap: round;
}
</style>
<script src="//d3js.org/d3.v3.min.js"></script>
<script src="//d3js.org/topojson.v1.min.js"></script>
<script>

var width = 960,
    height = 500;

var color = d3.scale.threshold()
    .domain([0.402, 0.422, 0.439, 0.461])
    .range(['#f0f9e8','#bae4bc','#7bccc4','#43a2ca','#0868ac']);

var path = d3.geo.path()
    .projection(null);

var svg = d3.select("div.post-content").append("svg")
    .attr("width", width)
    .attr("height", height);

d3.json("/data/income_inequality_map/us.json", function(error, us) {
  if (error) throw error;

  svg.append("g")
      .attr("class", "counties")
    .selectAll("path")
      .data(topojson.feature(us, us.objects.counties).features)
    .enter().append("path")
      .attr("d", path)
      .style("fill", function(d) { return color(d.properties.gini); });

  svg.append("path")
      .datum(topojson.mesh(us, us.objects.counties, function(a, b) { return a.id / 1000 ^ b.id / 1000; }))
      .attr("class", "state-borders")
      .attr("d", path);
});

</script>

---

## this post is a work in progress, please return later
