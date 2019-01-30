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
<p>Sound is an important feature of our human experience. Sound happens to be the largest medium of expression for humans, making music a universal language. Our culture and environment are filled with different patterns and rhythms of sounds. Our memories have engraved tones in them. We easily relate feelings with tones. We like to be heard or to hear.
 </p>

 <p>Today we will be looking into the musical scene in Nepal through the lens of Youtube. Youtube's platform is the biggest vibrant ecosystem for music creators and listeners. We have collected data on some of the most famous musical icons on youtube sphere and created a comparison chart on them. </p>

 <p>This is a one dimensional bar chart with a little twist to make it more musical. Each bar represents a channel and the length indicates the size of the subscriber. The proportionality of the graph is not an accurate representation of the data difference  as due to the difference in lowest to highest value in dataset. The graph does try to imitate the space of time in music so do expect some latency for the full graphical picture. But by the time you are done with this article the graph should already be drawn in the canvas below. Hover over the bars and you will get a textual information on the upper right hand side of the canvas which is a symlink for particular creators youtube space. </p>
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
var cluster = test.append("g");
var link = cluster.append("a");
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
	.on('mouseover',function(d,i){handleMouseOver(d.subscriber,d.musician,i,d.channelID)})
	.on("mouseout", function(d,i){handleMouseOut(d.subscriber)});
d3.selectAll("rect")
		.attr("fill","#FF5733")
		.transition()
		.delay(function(d,i){ return 3000*i; }) 
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
		.delay(function(d,i){ return 3000*i; }) 
		.duration(3000)
		.attr("transform",function(){
			return "translate("+ ((index*30)/2+60) +","+ (width/2 -20)+") rotate(90 0 0)";
		});
};
function remove(){
	d3.selectAll(".text").remove();
	d3.selectAll(".info").remove();
};
function handleMouseOver(subscriber,musician,i,channelID) {
	d3.selectAll(".visible").remove();
	link
		.attr("xlink:href", "https://www.youtube.com/channel/"+ channelID)
		.append("text")
		.attr("class", "visible")
		.attr("x", width/2)
		.attr("y", "40")
		.style("fill","white")
			.text(function(){
				return [musician + " has " + subscriber + " subscriber"]; 
			});
		};
function handleMouseOut(subscriber) {
	d3.select("#t" + subscriber).remove();
  };
</script>