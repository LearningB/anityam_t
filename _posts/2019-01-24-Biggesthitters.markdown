---
layout: post
title: "Biggest Hitters"
date: 2019-01-24
description: 
image: /assets/images/ten.jpg
author: B
tags: 
  - Top
  - Bubble
---
<p>This is the story of nepal youtube scene. The bubbles size represents the subscriber amount and the 
			color represents the scence.</p>
<button id="separate">Categorical Picture</button>
<button id="Overall">Whole Picture</button>
<div id="chart"></div>
<style>
button {
  background: transparent;
  border: 0;
  padding: 0;
  cursor: pointer;
  outline: 0;
  -webkit-appearance: none;
}
button {
  display: inline-block;
  position: relative;
  padding: 5px 4px;
  top: 0;
  font-size: 20px;
  font-family: "Open Sans", Helvetica;
  border-radius: 4px;
  border-bottom: 1px solid rgba(10, 224, 207, 0.733);
  background: rgb(22, 230, 168);
  color: #fff;
  box-shadow: 0px 0px 0px rgba( 15, 165, 60, 0.1 );
  -webkit-transform: translateZ(0);
     -moz-transform: translateZ(0);
      -ms-transform: translateZ(0);
          transform: translateZ(0);
  -webkit-transition: all 0.2s ease;
     -moz-transition: all 0.2s ease;
      -ms-transition: all 0.2s ease;
          transition: all 0.2s ease;
}
button:hover {
  top: -10px;
  box-shadow: 0px 10px 10px rgba( 15, 165, 60, 0.2 );
  -webkit-transform: rotateX(20deg);
     -moz-transform: rotateX(20deg);
      -ms-transform: rotateX(20deg);
          transform: rotateX(20deg);
}
button:active {
  top: 0px;
  box-shadow: 0px 0px 0px rgba( 15, 165, 60, 0.0 );
  background: rgba( 20, 224, 133, 1 );
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
  font-size : 10px;
}
.swatch{
  height: 10px;
  width: 10px;
}
.legendTitle{
  font-size :10px;
}
</style>
<script src="https://d3js.org/d3.v4.min.js"></script>
<script src="https://unpkg.com/d3-force-attract@latest"></script>
<script src="https://unpkg.com/d3-force-cluster@latest"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/d3-legend/2.13.0/d3-legend.js"></script>
<script>
margin= { top: 20, right: 25, bottom: 20, left: 25 };
width = window.innerWidth - margin.left - margin.right;
height = 900 - margin.top - margin.bottom;
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
	.attr("viewBox", "0 0 800 900")
	.append("g")
  .attr("transform","translate(0,0)");		
var radiusScale = d3.scaleSqrt().domain([10, 5000000]).range([1,50]);
var ordinalScale = d3.scaleOrdinal()
		.domain(function(d){
			return d.category.toLowerCase()
		}).range(['#ff0000', '#ffad33', '#09d9ff']);	    
var forceX = d3.forceX(function(d){
	if (d.category === "vlogger"){
		return 100 
	}else if (d.category === "musicChannel"){
		return 300
	}else{
		return 500
	}
	}).strength(0.5);
var together = d3.forceX(function(){
	return width/2
	}).strength(0.05);
var collides = d3.forceCollide(function(d){
	return radiusScale(d.subscriber)+3;
	});
var simulation = d3.forceSimulation()
    .force('center', d3.forceCenter(width/2, height/3))
	.force("x", together)
	.force("y", d3.forceY(height/2).strength(0.05))
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
    dataIndex = [10,25,50];
    redraw(dataIndex);
    var legend = svg.append("g")
        .attr("class", "legendOrdinal")
        .attr("transform", "translate(10,10)");
    var legendOrdinal = d3.legendColor()
                .scale(legendColor)
                .orient("vertical")
                 .title("Color shows the channels category");
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
                .force("x", d3.forceX(width/2).strength(0.25))
				.alphaTarget(0.05)
                .restart()
            })
			.transition(t);			
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
        .text("The size of the bubble represents the number of subscriber")
        .style("font-size","10px")
        .attr("transform", "translate(280,20)");
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
        .attr("transform", "translate(240,50) rotate(-90 80 80)")
        .style("stroke-dasharray", ("2,1")) 
        .style("stroke", "black")
        .style("fill", "none")
        .text("The size defines subscriber");
}
</script>