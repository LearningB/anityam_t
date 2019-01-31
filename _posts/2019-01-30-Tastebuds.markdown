---
layout: post
title: "Taste Buds"
date: 2019-01-30
description: 
image: /assets/images/flavor.jpg
author: Bijendra
tags: 
  - SAy
---
<p>Food </p>
<div id="chart"></div>
<script src="https://d3js.org/d3.v4.min.js"></script>
<script src="http://vizjs.org/viz.v1.3.0.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/d3-legend/2.13.0/d3-legend.js"></script>
<script>
width = 700;
height = 700;
var mainData=[];
var meatData=[];
var views=[];
d3.queue()
	.defer(d3.json,"/data/food.json")
	.await(start);
var color = {sajilokitchen:"#3366CC", kukmandu:"#DC3912", learntocookwithme:"#FF9900",yummyfoodworld:"#109618",yummynepalikitchen:"#990099",chefsuni:"#0099C6"};
var svg = d3.select("#chart").append("svg")
        .attr("viewBox", "0 0 900 800")
        .attr("width", 960).attr("height", 800);
svg.append("text").attr("x",200).attr("y",170)
	.attr("class","header").text("Veg or Non Veg");
svg.append("text").attr("x",650).attr("y",70)
	.attr("class","header").text("Meat");
var legendColor = d3.scaleOrdinal()
		.domain(["Sajilo Kitchen", "Kukmandu", "Learn To Cook With Me","Yummy Food World","Yummy Nepali Kitchen","Chef Suni" ])
        .range(["#3366CC", '#DC3912', "#FF9900","#109618","#990099","#0099C6"]);
function start(error,data){
    data.forEach(function(d){
        views.push(parseInt(d.viewCount));
		var cell = [];
		cell.push((d.name).toLowerCase());
		cell.push(d.dish);
		cell.push(+d.viewCount);
		cell.push(d.totalCount);
		cell.push(d.type.toLowerCase());
		mainData.push(cell);
		if (d.dish.toLowerCase() === "meat"){
			var newCell = [];
			newCell.push((d.name).toLowerCase());
			newCell.push(d.item);
			newCell.push(+d.viewCount);
			meatData.push(newCell);
		};
        });
    console.log(data);
	var scale = d3.scaleLog()
			.domain([d3.min(views), d3.max(views)])
			.range([1,20]);
	for (i = 0; i < meatData.length; i++) { 
			meatData[i][2] = scale(meatData[i][2])
		};
	for (i = 0; i < mainData.length; i++) { 
		mainData[i][2] =scale(mainData[i][2])
	};
	var g =[svg.append("g").attr("transform","translate(150,40)")
		,svg.append("g").attr("transform","translate(650,100)"),
		svg.append("g").attr("transform","translate(150,400)")];
	var bp=[viz.biPartite()
		.data(mainData)
		.min(10)
		.pad(1)
		.height(300)
		.width(200)
		.barSize(49)
		.orient("vertical")
		.edgeMode("straight")
		.fill(d=>color[d.primary]),
		viz.biPartite()
		.data(meatData)
		.keyPrimary(d=>d[0])
		.keySecondary(d=>d[1])
		.value(d=>d[2])
		.min(10)
		.pad(1)
		.height(400)
		.width(200)
		.barSize(50)
		.orient("vertical")
		.edgeMode("straight")
		.fill(d=>color[d.primary]),
		viz.biPartite()
		.data(mainData)
		.keyPrimary(d=>d[0])
		.keySecondary(d=>d[4])
		.value(d=>d[2])
		.min(10)
		.pad(1)
		.height(400)
		.width(200)
		.barSize(50)
		.orient("vertical")
		.edgeMode("straight")
		.fill(d=>color[d.primary])
	];
[0,1,2].forEach(function(i){
	g[i].call(bp[i]);
	g[i].selectAll(".viz-biPartite-mainBar")
	.on("mouseover",mouseover)
	.on("mouseout",mouseout);
	g[i].selectAll(".viz-biPartite-mainBar").append("text").attr("class","label")
		.attr("x",d=>(d.part=="primary"? -30: 30))
		.attr("y",d=>+6)
		.text(d=>d.key)
		.attr("text-anchor",d=>(d.part=="primary"? "end": "start"));
g[i].selectAll(".viz-biPartite-mainBar").append("text").attr("class","perc")
	.attr("x",d=>(d.part=="primary"? 10: -10))
	.text(function(d){ return d3.format("0.0%")(d.percent)})
	.attr("text-anchor",d=>(d.part=="primary"? "end": "start"));
	function mouseover(d){
		bp[i].mouseover(d);
		g[i].selectAll(".viz-biPartite-mainBar")
		.select(".perc")
		.text(function(d){ return d3.format("0.0%")(d.percent)})
	};
	function mouseout(d){
		bp[i].mouseout(d);
		g[i].selectAll(".viz-biPartite-mainBar")
			.select(".perc")
		.text(function(d){ return d3.format("0.0%")(d.percent)})
	};
})
};
</script>