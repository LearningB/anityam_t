---
layout: post
title: "Looking Through A Crowd"
date: 2019-01-24
description: 
image: /assets/images/say-something.jpg
author: Biwash
tags: 
  - SAy
  - BuBble
---
<p>Creativity is just a dare to move along and do something. Its not an idea of ingenuity, modernity or being fresh and new. It's a simple concept of having a dream, searching it and publishing that dream for others to see. Creativity is just a statement. </p>

<p>
On this very first article I am honouring few creative heads in Nepalese youtube space. Below is a bubbly graphical floatation of some creative channel creators of Nepal. Each bubble represents a channel and the size represents the users that have bought the idea of that channel i.e. subscribers. </p>
<p>
Now looking through the perspective of a data visualizer, the bubble chart is not considered to be the best representation of size but this view is not a comparison of personality but just a simple sample on the youtube space in Nepal. Use the buttons below to toggle between categorical and whole picture. Hover through the bubbles to discover the people and their followers.</p>

<button id="separate">Categorical Picture</button>
<button id="Overall">Whole Picture</button>
<div id="chart"></div>
<style>
button {
  background-color: #4CAF50;
  border: none;
  color: white;
  width: 30%;
  padding: 5px 2px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 10px;
}
button:hover {
 background-color: #3e8e41;
 }
button:active {
  background-color: #3e8e41;
}
div.tooltip {
  display: block;
  position: absolute;
  text-align: center;
  width: 300px;
  height: 30px;
  padding: 2px;
  font: 12px sans-serif;
  background: lightsteelblue;
  border: 0px;
  border-radius: 8px;
  pointer-events: none;
}
.label{
  font-size : 20px;
}
.swatch{
  height: 20px;
  width: 20px;
}
.legendTitle{
  font-size :0px;
}
svg{
  height: 500px;
  width: 700px
}
</style>
<script src="https://d3js.org/d3.v4.min.js"></script>
<script src="https://unpkg.com/d3-force-attract@latest"></script>
<script src="https://unpkg.com/d3-force-cluster@latest"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/d3-legend/2.13.0/d3-legend.js"></script>
<script>
margin= { top: 40, right: 25, bottom: 20, left: 25 };
width = 700 - margin.left - margin.right;
height = 700 - margin.top - margin.bottom;
var t = d3.transition()
        .duration(200)
        .delay(function(d, i) { return i * 5; })
        .ease(d3.easeLinear);
var v = d3.transition()
		.duration(900)
        .ease(d3.easeLinear);
var x = d3.scalePoint()
		.domain(["Vlogger", "Music Channel", "Web Channel"])
        .range([0,width]);        
var legendColor = d3.scaleOrdinal()
		.domain(["Vlogger", "Music Channel", "Web Channel"])
		.range(["#ff0000", '#ffad33', '#09d9ff']);
var svg = d3.select("#chart").append("svg")
	.attr("viewBox", "0 0 700 500")
	.append("g")
  .attr("transform","translate(0,0)");
var chartDiv = document.getElementById("chart");		
var radiusScale = d3.scaleSqrt().domain([10, 5000000]).range([1,50]);
var ordinalScale = d3.scaleOrdinal()
		.domain(function(d){
			return d.category.toLowerCase()
		}).range(['#ff0000', '#ffad33', '#09d9ff']);	    
var forceX = d3.forceX(function(d){
	if (d.category === "vlogger"){
		return 100
	}else if (d.category === "musicChannel"){
		return 200
	}else{
		return 500
	}
	}).strength(1);
var together = d3.forceX(function(){
	return width/2
	}).strength(0.05);
var collides = d3.forceCollide(function(d){
	return radiusScale(d.subscriber)+3;
	});
var simulation = d3.forceSimulation()
    .force('center', d3.forceCenter(width/2, height/3))
	.force("x", together)
	.force("y", d3.forceY(height/2).strength(0.03))
    .force("collide",collides);
var transitionTime = 3000;
var t = d3.timer(function (elapsed) {
  var dt = elapsed / transitionTime;
  simulation.force('collide').strength(Math.pow(dt, 2) * 0.7);
  if (dt >= 1.0) t.stop();
});
d3.queue()
	.defer(d3.json, "/data/top.json")
	.await(ready)	;
var div = d3.select("#chart").append("div")
	.attr("class", "tooltip")
	.style("z-index", "10")
    .style("opacity", 0);
function ready(error,data){
  var widths = chartDiv.clientWidth;
    dataIndex = [10,25,50];
    redraw(dataIndex);
    var legend = svg.append("g")
        .attr("class", "legendOrdinal")
        .attr("transform", "translate(-10,0)");
    var legendOrdinal = d3.legendColor()
                .scale(legendColor)
                .orient("vertical")
                 .title("");
    svg.select(".legendOrdinal")
        .call(legendOrdinal);
    var circles = svg.selectAll(".artist")
		.data(data)
		.enter().append("circle")
		.attr("class","artist")
		.attr("r",function(d){
			return radiusScale(d.subscriber)
		})
		.attr("fill",function(d){
		    return ordinalScale(d.category.toLowerCase())
		})
		.on('mouseover',function(d){
			d3.select(this)
    	    .transition()
    		.attr('r', function(d){
				return radiusScale(d.subscriber) *1.5
			})
			.attr('stroke', 'black');
			div.transition()
			.duration(3)
			.style("opacity", .9);
		 div.html( d.name +" : " + d.subscriber + " subscriber")
			.style("left", (d3.event.pageX) + "px")
			.style("top", (d3.event.pageY - 28) + "px");
		})
			.on('mouseout',function(d){
			d3.select(this)
    		.transition()
    		.attr('r', function(d){
				return radiusScale(d.subscriber)
			})
			.attr('stroke', '');
			div.transition()
				  .duration(500)
				  .style("opacity", 0);
			});
    d3.select("#separate")
		.on('click',function(){
             svg.selectAll(".index").remove();
             svg.selectAll(".textIndex").remove();
            simulation
				.force("x", forceX)
        .alphaTarget(0.05)
				.restart()
				})
        .transition(v);          
	d3.select("#Overall")
		.on('click', function(){
            redraw(dataIndex);
			simulation
                .force("x", d3.forceX(widths/2).strength(0.25))
				.alphaTarget(0.05)
                .restart()
            })
			.transition(v);			
	simulation.nodes(data)
        .on("tick",ticked);
function ticked(){
		circles
		    .attr("cx", function(d){
				return d.x
		   	})
			.attr("cy",function(d){
				return d.y
			});
		}		
}
function redraw(data){
    svg.selectAll(".textIndex")
        .data([1])
        .enter()
        .append("text")
        .attr("class", "textIndex")
        .text("Size = number of Subscriber")
        .style("font-size","20px")
        .attr("transform", "translate(410,0)");
    svg.selectAll(".index")
        .data(data)
        .enter().append("circle")
        .attr("cx" ,function(d){
            return 100 + d;
        })
        .attr("class", "index")
        .attr("r", function(d){
            return d
        })
        .attr("transform", "translate(500,70) rotate(-90 80 80)")
        .style("stroke-dasharray", ("2,1")) 
        .style("stroke", "black")
        .style("fill", "none")
        .text("The size defines subscriber");
}
</script>