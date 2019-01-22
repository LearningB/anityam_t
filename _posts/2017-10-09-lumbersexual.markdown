---
layout: post
title: "Cluster Data"
date: 2019-01-21
description: 
image: /assets/images/placeholder-3.jpg
author: Bijen
tags:
  - Mixtape
  - Moon Drinking
  - Kale
---
This is the first trial blog for the first project. A simple showcase on how I want to gather and visualize data. As per the below demo chart, it's basically a cluster view of channels and their videos. Each color represents a specific channel and the different sized bubbles for that color represents videos produced by those channel. The shape of the bubble is dictated by the total number of views on that video, the bigger the bubble the larger the audience. 


<div id="chart"></div>
<style>
  svg {
  display: block;
  margin: auto;
  border: 1px solid gray;
}
</style>
<script>
var width = window.innerWidth,
    height = 500,
    padding = 1.5, 
    clusterPadding = 6, 
    maxRadius = 12;
var n = 200, 
    m = 10; 
var color = d3.scale.category10()
    .domain(d3.range(m));
var clusters = new Array(m);
var nodes = d3.range(n).map(function() {
  var i = Math.floor(Math.random() * m),
      r = Math.sqrt((i + 1) / m * -Math.log(Math.random())) * maxRadius,
      d = {cluster: i, radius: r};
  if (!clusters[i] || (r > clusters[i].radius)) clusters[i] = d;
  return d;
});
var force = d3.layout.force()
    .nodes(nodes)
    .size([width, height])
    .gravity(0)
    .charge(0)
    .on("tick", tick)
    .start();
var svg = d3.select("#chart").append("svg")
    .attr("width", width, "100%")
    .attr("height", height,"100%");
var circle = svg.selectAll("circle")
    .data(nodes)
  .enter().append("circle")
    .attr("r", function(d) { return d.radius; })
    .style("fill", function(d) { return color(d.cluster); })
    .call(force.drag);
function tick(e) {
  circle
      .each(cluster(10 * e.alpha * e.alpha))
      .each(collide(.5))
      .attr("cx", function(d) { return d.x; })
      .attr("cy", function(d) { return d.y; });
}
function cluster(alpha) {
  return function(d) {
    var cluster = clusters[d.cluster],
        k = 1;
    if (cluster === d) {
      cluster = {x: width / 2, y: height / 2, radius: -d.radius};
      k = .1 * Math.sqrt(d.radius);
    }
  var x = d.x - cluster.x,
        y = d.y - cluster.y,
        l = Math.sqrt(x * x + y * y),
        r = d.radius + cluster.radius;
    if (l != r) {
      l = (l - r) / l * alpha * k;
      d.x -= x *= l;
      d.y -= y *= l;
      cluster.x += x;
      cluster.y += y;
    }
  };
}
function collide(alpha) {
  var quadtree = d3.geom.quadtree(nodes);
  return function(d) {
    var r = d.radius + maxRadius + Math.max(padding, clusterPadding),
        nx1 = d.x - r,
        nx2 = d.x + r,
        ny1 = d.y - r,
        ny2 = d.y + r;
    quadtree.visit(function(quad, x1, y1, x2, y2) {
      if (quad.point && (quad.point !== d)) {
        var x = d.x - quad.point.x,
            y = d.y - quad.point.y,
            l = Math.sqrt(x * x + y * y),
            r = d.radius + quad.point.radius + (d.cluster === quad.point.cluster ? padding : clusterPadding);
        if (l < r) {
          l = (l - r) / l * alpha;
          d.x -= x *= l;
          d.y -= y *= l;
          quad.point.x += x;
          quad.point.y += y;
        }
      }
      return x1 > nx2 || x2 < nx1 || y1 > ny2 || y2 < ny1;
    });
  };
}
</script>


