---
layout: post
title: "Finding A Note"
date: 2019-01-29
description: 
image: /assets/images/note.jpg
author: Bijendra
tags: 
  - SAy
  - Bar
---
<p></p>
<div id="chart"></div>
<script src="https://d3js.org/d3.v4.min.js"></script>
<script>
width = 700;
height = 700;
var test = d3.selectAll("#chart")
            .append("svg")
            .attr("viewBox", "0 0 700 700")
			.attr("height", height)
			.attr("width", width)
			.attr("fill", "#FF5733")
			.style("background","#FF5733");
d3.queue()
	.defer(d3.json, "/data/music.json")
	.await(create);
var scale = d3.scaleLinear()
	.domain([0,438978]).range([10,100]);
function create(error,data){
	 test.selectAll("rect")
		.data(data)
		.enter()
		.append("rect")
		.attr("class", "music")
		.attr("rx","5")
		.attr("ry", "5")
		.attr("width", function(d){
			return scale(d.subscriber)*3;
	})
	.attr("height","10")
	.on('mouseover',function(d,i){handleMouseOver(d.subscriber,d.musician,i)})
	.on("mouseout", function(d,i){handleMouseOut(d.subscriber)});
d3.selectAll("rect")
		.attr("fill","#FF5733")
		.transition()
		.delay(function(d,i){ return 6000*i; }) 
		.duration(3000)
		.attr("transform",function(d, i){
			return "translate("+ ((i*30)/2+30) +","+ (width/2 -(scale(d.subscriber)*3)/2)+") rotate(90 0 0)";
		})
		.attr("fill","black")
		.on("start", function(d,i){
			draw(d.musician,i);
		})
		.on("end", function(d){
			remove();
		});
};
function draw(name, index){
	var elemEnter = test
		.append("g");
	elemEnter.append("text")
		.attr("dy", ".35em")
		.attr("class", "text")
		.attr("x", "10")
		.attr("y", "10")
		.attr("font-family", "sans-serif")
		.attr("fill", "black")
		.text(name)
		.transition()
		.delay(function(d,i){ return 6000*i; }) 
		.duration(3000)
		.attr("transform",function(){
			return "translate("+ ((index*30)/2+60) +","+ (width/2 -20)+") rotate(90 0 0)";
		});
};
function remove(){
	d3.selectAll(".text").remove();
	d3.selectAll(".info").remove();
};
function handleMouseOver(subscriber,musician,i) {  
	test.append("text")
	.attr("id", function(){
		return  "t" + subscriber;
	})
	.attr("x", function(){
		return  ((i*30)/2+30) - 20;
	})
	.attr("y", function(){
		return  (width/2 -(scale(subscriber)*3)/2) - 15;
	})
	.text(function() {
	  return [musician + " : " + subscriber]; 
	})
	.attr("fill", "#cc0e08");
  }
function handleMouseOut(subscriber) {
	d3.select("#t" + subscriber).remove();
  };
</script>