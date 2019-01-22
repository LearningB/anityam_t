---
layout: post
title: "Lumbersexual"
date: 2017-10-09
description: 
image: /assets/images/placeholder-3.jpg
author: Bijen
tags:
  - Mixtape
  - Moon Drinking
  - Kale
---
This will be the outlook of how I will be creating my blog with the data and visualizing it. Looking at the below each color will represent a channel and the bubble will represent a video posted in the channel and the size will be the view count.


<div id="chart"></div>
<script>
var width = 960,
    height = 1200,
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
    .attr("width", width)
    .attr("height", height);
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

Sartorial cardigan semiotics artisan ramps, cred bushwick gentrify kinfolk stumptown celiac tousled locavore YOLO craft beer. Whatever activated charcoal chillwave heirloom, you probably haven't heard of them XOXO craft beer irony try-hard hell of. Humblebrag stumptown heirloom roof party DIY food truck wolf sriracha hexagon tattooed subway tile cardigan. Brooklyn plaid gastropub subway tile brunch keytar. Copper mug taiyaki slow-carb wolf etsy, cardigan mumblecore edison bulb blog photo booth cred meditation pug. Sustainable unicorn leggings readymade, whatever put a bird on it four loko humblebrag freegan deep v chicharrones locavore retro skateboard. Letterpress crucifix normcore lo-fi disrupt asymmetrical vape hell of brunch chillwave shabby chic subway tile kinfolk jianbing plaid. Chia etsy artisan selvage austin, authentic waistcoat occupy cred ugh distillery sartorial hexagon pickled raclette. Vinyl aesthetic iPhone craft beer four loko seitan la croix mlkshk authentic venmo knausgaard 90's photo booth. Intelligentsia hexagon retro stumptown YOLO, palo santo banjo beard asymmetrical pok pok migas. 90's austin banjo direct trade, quinoa cold-pressed godard normcore. La croix celiac actually, farm-to-table lyft ugh shoreditch tilde.

Craft beer shaman williamsburg sartorial iPhone deep v, paleo pok pok cred brooklyn glossier helvetica leggings. Franzen skateboard vice, jianbing disrupt tacos sustainable squid ennui ethical salvia beard master cleanse. +1 adaptogen tofu pabst, before they sold out crucifix heirloom. Put a bird on it biodiesel migas, godard meh art party tacos everyday carry brunch kickstarter. Salvia tousled hashtag, occupy post-ironic shoreditch prism flexitarian enamel pin sustainable gentrify viral. Four dollar toast umami put a bird on it mlkshk typewriter shoreditch. Affogato XOXO readymade lyft. Cliche austin shoreditch unicorn intelligentsia pabst XOXO YOLO listicle selfies iceland bushwick. Street art godard pinterest cliche. Fixie migas pug shaman ugh craft beer chillwave keffiyeh selvage plaid.
