---
layout: post
title: "Another Bar"
date: 2019-01-16
description: 
image: /assets/images/placeholder-15.jpg
author: B
tags:
  - Bar
  - Chart
---
This is a bar chart that is aligned and placed properly and also is using data from data file.I don't know what this data is about but it's cool.

<h3>D3.js Bar Chart Using YAML and Jekyll</h3>
<p>This is a D3.js bar chart that is driven from dynamically generated JSON, from YAML stored in the _data folder within this Github Pages repository running Jekyll.</p>
<div id="chart"></div>
<p>The YAML can be found in <a href="https://github.com/kinlane/d3-js-using-yaml-jekyll/tree/gh-pages/_data" target="_blank">_data/bar-chart.yaml</a>, but is transformed into the JSON needed for this chart, using <a href="https://github.com/kinlane/d3-js-using-yaml-jekyll/blob/gh-pages/data/bar-chart.json" target="_blank">/data/bar-chart.json</a>, demonstrating how YAML can be used to drive visualizations on Github.</p>
<style>
/* Bar Chart */
.bar {
  fill: steelblue;
}
.bar:hover {
  fill: brown;
}
.axis {
  font: 10px sans-serif;
}
.axis path,
.axis line {
  fill: none;
  stroke: #000;
  shape-rendering: crispEdges;
}
.x.axis path {
  display: none;
}
</style>
<script src="//d3js.org/d3.v3.min.js"></script>
<script>
var margin = {top: 20, right: 20, bottom: 30, left: 40},
    width = window.innerWidth - margin.left - margin.right,
    height = 500 - margin.top - margin.bottom;
var x = d3.scale.ordinal()
    .rangeRoundBands([0, width], .1);
var y = d3.scale.linear()
    .range([height, 0]);
var xAxis = d3.svg.axis()
    .scale(x)
    .orient("bottom");
var yAxis = d3.svg.axis()
    .scale(y)
    .orient("left")
    .ticks(10);
var svg = d3.select("#chart").append("svg")
    .attr("width", width + margin.left + margin.right)
    .attr("height", height + margin.top + margin.bottom)
  .append("g")
    .attr("transform", "translate(" + margin.left + "," + margin.top + ")");
d3.json("/data/bar-chart.json", function(error, data) {
  if (error) throw error;
  x.domain(data.map(function(d) { return d.letter; }));
  y.domain([0, d3.max(data, function(d) { return d.frequency; })]);
  svg.append("g")
      .attr("class", "x axis")
      .attr("transform", "translate(0," + height + ")")
      .call(xAxis);
  svg.append("g")
      .attr("class", "y axis")
      .call(yAxis)
    .append("text")
      .attr("transform", "rotate(-90)")
      .attr("y", 6)
      .attr("dy", ".71em")
      .style("text-anchor", "end")
      .text("Number of Years");
  svg.selectAll(".bar")
      .data(data)
    .enter().append("rect")
      .attr("class", "bar")
      .attr("x", function(d) { return x(d.letter); })
      .attr("width", x.rangeBand())
      .attr("y", function(d) { return y(d.frequency); })
      .attr("height", function(d) { return height - y(d.frequency); });
});
function type(d) {
  d.frequency = +d.frequency;
  return d;
}
</script>




Jean shorts pour-over chicharrones woke. Kinfolk next level chia master cleanse. Messenger bag green juice tumeric trust fund pour-over vegan. Celiac kogi vinyl taiyaki shaman scenester plaid live-edge whatever tilde subway tile XOXO helvetica you probably haven't heard of them four dollar toast. Knausgaard franzen mumblecore normcore microdosing. Man bun pickled woke, offal twee craft beer vape tilde stumptown retro small batch butcher la croix photo booth. 

Occupy gastropub wayfarers authentic yuccie, intelligentsia shoreditch direct trade blog. Hell of neutra gochujang knausgaard irony normcore enamel pin. Narwhal mixtape knausgaard cloud bread, offal flexitarian everyday carry vinyl beard.

Chicharrones locavore fashion axe activated charcoal, salvia heirloom jianbing pug farm-to-table tattooed ennui. Wayfarers poke edison bulb banjo street art normcore. Gentrify hashtag health goth listicle biodiesel chambray wolf vape. Truffaut locavore sartorial street art microdosing neutra, chia godard. Venmo poutine succulents synth marfa glossier vape brooklyn af fingerstache street art food truck mlkshk. Twee trust fund etsy bicycle rights umami, brunch ennui mlkshk poutine. Ennui ugh cred distillery. Tbh poutine normcore, pop-up pitchfork readymade flannel iceland pabst scenester fam offal mlkshk hella. 

Hella gastropub dreamcatcher tumblr lumbersexual kinfolk. Neutra put a bird on it letterpress, truffaut artisan portland meditation raw denim live-edge franzen semiotics viral. Neutra selfies knausgaard green juice raclette. Blue bottle disrupt 8-bit coloring book farm-to-table seitan. Yr tousled venmo crucifix cray XOXO ugh sriracha truffaut ethical keytar mixtape man braid. Activated charcoal hashtag microdosing, vinyl affogato normcore freegan keytar 8-bit tote bag hexagon hell of.

![Placeholder](/assets/images/placeholder-23.jpg#full)

Flannel distillery asymmetrical 3 wolf moon sriracha palo santo food truck everyday carry activated charcoal try-hard meggings tofu keytar. Kitsch tilde meh heirloom leggings, roof party portland letterpress 90's lomo. Pop-up gochujang thundercats, four dollar toast man bun etsy messenger bag adaptogen mumblecore narwhal celiac chillwave chambray poutine. Vaporware craft beer occupy tattooed authentic cray. Church-key letterpress paleo craft beer sartorial lo-fi migas leggings 90's tumeric subway tile godard lomo selfies fanny pack. Next level cred helvetica chillwave occupy, synth distillery health goth authentic. 

![Placeholder](/assets/images/placeholder-29.jpg#full)

Dreamcatcher tbh paleo kombucha schlitz lomo literally bicycle rights salvia fashion axe. Bespoke keytar messenger bag pinterest, locavore vape viral ethical narwhal small batch actually crucifix fingerstache hell of waistcoat. 

![Placeholder](/assets/images/placeholder-2.jpg)

Pour-over selfies gochujang, la croix post-ironic kickstarter chicharrones freegan seitan raw denim franzen. Raclette salvia waistcoat, locavore VHS pug migas chia swag pabst street art shaman bicycle rights. Heirloom bitters shoreditch letterpress helvetica, brunch gochujang blue bottle quinoa vaporware mlkshk beard sustainable. Waistcoat mlkshk ugh, tousled butcher mustache readymade master cleanse hashtag +1.

Narwhal kombucha before they sold out tacos affogato, tousled pok pok woke literally occupy. Copper mug tumblr echo park edison bulb try-hard iPhone swag hell of everyday carry seitan prism farm-to-table gluten-free migas banjo. Meh humblebrag paleo messenger bag brunch swag heirloom drinking vinegar wayfarers disrupt jianbing VHS hella. Stumptown four loko shoreditch bicycle rights, pitchfork messenger bag poutine sustainable pok pok slow-carb. Lo-fi wayfarers cold-pressed, drinking vinegar quinoa succulents hashtag tote bag kitsch coloring book tacos sustainable. Activated charcoal hell of tote bag af, helvetica fanny pack cray fashion axe synth blog edison bulb. Subway tile iceland banh mi pickled air plant. Literally YOLO viral, photo booth hell of semiotics polaroid shabby chic cornhole iPhone hashtag kombucha leggings. Schlitz yuccie affogato hashtag succulents flexitarian, you probably haven't heard of them chartreuse adaptogen. Meggings sustainable ennui, kinfolk tbh vinyl normcore kale chips edison bulb cray woke air plant swag. Neutra cardigan palo santo whatever.
