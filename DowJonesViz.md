---
layout: null
title: Dow Jones Visualization  
permalink: /DowJonesViz/
---
<!DOCTYPE html>
<meta charset="utf-8">
<html>
<HEAD>
    <link href='https://fonts.googleapis.com/css?family=Roboto' rel='stylesheet' type='text/css'>
    <link href='https://fonts.googleapis.com/css?family=Alegreya' rel='stylesheet' type='text/css'>
    <script src="https://d3js.org/d3.v4.min.js"></script>
    <script src="https://d3js.org/d3-color.v1.min.js"></script>
    <script src="https://d3js.org/d3.v3.min.js"></script>
    <!-- <script src="http://d3js.org/queue.v1.min.js"></script>
    <script src="https://d3js.org/d3-interpolate.v1.min.js"></script>
    <script src="http://labratrevenge.com/d3-tip/javascripts/d3.tip.v0.6.3.js"></script> -->

</HEAD>

<style>

form {
  font-family: "Roboto", Helvetica, Arial, sans-serif
  font-size: 12px;
}

body {
    font-family: "Alegreya"
}

svg {
  font-size: 12px;
  font-weight: 900;
  font-family: "Roboto", Helvetica, Arial, sans-serif;
}

.axis path,
.axis line {
  fill: none;
  stroke: #000;
  shape-rendering: crispEdges;
}

.dot {
  stroke: #000;
}

#tooltip {
  position: absolute;
  width: 160px;
  height: auto;
  padding: 5px;
  background-color: lightsteelblue;
  -webkit-border-radius: 10px;
  -moz-border-radius: 10px;
  border-radius: 10px;
  -webkit-box-shadow: 4px 4px 10px rgba(0, 0, 0, 0.4);
  -moz-box-shadow: 4px 4px 10px rgba(0, 0, 0, 0.4);
  box-shadow: 4px 4px 10px rgba(0, 0, 0, 0.4);
  /*pointer-events: none;*/
  cursor: pointer;
  font-family: "Roboto", Helvetica, Arial, sans-serif;
  font-size: 12px;
  font-weight: 900;
}

#tooltip.hidden {
  display: none;
}

</style>
<body>
    <h1>Understanding the Dow Jones Industrial Average</h1>
    <h2>A Visualization by Sarah Swanz</h2>

<p>
    The Dow Jones Industrial Average is a commonly-used indicator of stock market performance in the United States. The Dow is an average of the prices of 30 large publicly-traded companies divided by the Dow Divisor. The Dow Divisor is adjusted whenever there are stock splits, mergers or spinoffs, and changes among the Dow component companies. This helps maintain the  continuity of the index and avoid the distortion such changes would otherwise cause. The Dow is a proprietary index first developed in 1896 and now owned by S&amp;P Dow Jones Indices.
</p>
<p>
    The Dow is a price-weighted average of its component stocks. As a price-weighted average, it gives higher-priced stocks more influence over the average than lower-priced stocks, and does not take into account the relative market capitalization of its component companies. For example, a $1 increase in a $10 stock can be negated by a $1 decrease in a $100 stock, even though the $10 stock had a 10% increase and the $100 stock had merely a 1% increase.</p>
<p>
    Boeing and Goldman Sachs currently are two of the highest priced stocks in the Dow and therefore have the greatest influence on it. On the other hand, Cisco Systems and General Electric are two of the lowest priced stocks in the Dow and have the least amount of influence on the price movement. The weight of each stock in the Dow is a measure of its influence. </p>
<p>
    Some critics advocate for a change in the weighting of Dow components to better reflect their market capitalization (the number of shares outstandings x the price). If each stock was weighted equally, then their weights would all be 3.33%.</p>
<p>
    The first chart below shows the weights of the component stocks of the Dow in its current  price-weighted form and compares them what their weights would be if the Dow were weighted by market capitalization instead. You can hover over the dot to get the exact weight.
</p>

<!--  insert first chart-->

<svg id="plotSVG" width="960" height="570"></svg>
<div id="tooltip" class="hidden">
  <p><strong id="heading"></strong></p>
  <p><span id="percentage"></span></p>
  <p><span id="revenue"></span></p>
</div>

<p>
    You may have noticed that some stocks, like Visa and Chevron, would not have changed very much if the weightings changed. And their weights are very close to what it would be if the stocks were equally weighted.
</p>
<p>
    On the other hand, some stocks like Apple and Microsoft would see a huge change in their influence over the Dow. In fact, of the top six companies that would see the largest increase in their weights, four are in the technology sector. (By the way, Apple only joined the Dow in 2015, replacing AT&amp;T.)
</p>
<p>
    In the next chart, you will see how much the Technology sector would influence the Dow if the Average became weighted by market capitalization. You will also see how this change would come at the expense  of the Industrial and Financial Sectors. You can decide for yourself whether you think current Dow over-emphasizes certain sectors or stocks. Feel free to hover over a block and see the different weightings for that company.
</p>
<!--  insert second chart-->
<form>
  <label><input type="radio" name="mode" value="sumBySize" checked> Show Current Weight by Price</label>
  <label><input type="radio" name="mode" value="sumByCount"> Show Weighting by Market Cap</label>
</form>
<p></p>

<svg id="legendSVG" width="960" height="20"></svg>
<svg id="treeSVG" width="960" height="570"></svg>
<p>Data as of November 10, 2017. Stock market data retrieved from Yahoo Finance.</p>
<svg  width="960" height="100"></div>
<p>Copyright: Sarah Swanz 2017</p>

<script src="https://d3js.org/d3.v4.min.js"></script>
<script type="text/javascript">

// Scatterplot of 3 weightings
// With help from https://bl.ocks.org/mbostock/3887118

var margin = {top: 20, right: 60, bottom: 30, left: 60},
    width = 960 - margin.left - margin.right,
    height = 675 - margin.top - margin.bottom;

var yStartLoc = 20;
// var spaceBetweenCos=20;
// var yLoc = 10 + spaceBetweenCos;
var xStartLoc = 55;



var plotDiv = d3.select("#plotSVG")
    .attr("width", width + margin.left)  // + margin.right
    .attr("height", height + margin.top + margin.bottom)
    .append("g")
    .attr("transform", "translate(" + margin.left + "," + margin.top + ")");


d3.json("ProjectData2.json", function(error, data) {
  if (error) throw error;

  data.forEach(function(d) {
    d.Name = d.Ticker;
    d.WeightDJ = +d.WeightDJ;
    // d.WeightB = +d.WeightB;
    d.WeightMC = +d.WeightMC;
  });

  var y = d3.scale.linear()
    .domain([30, 0])
    .range([height, 0]);

  var x = d3.scale.linear()
      // .domain(d3.extent(data, function(d) {return d.WeightMC; }))
      .domain([0, 15])  // don't hard code - use data
      .range([0, width - 150]);

  var groups = plotDiv.selectAll('g')
    .data(data)
    .enter()
    .append('g');

  groups.append("circle")  // set up x,y coordinates of WeightDJ - current index
      .attr("class", "dot")
      .attr("r", 3.5)
      .attr("cx", function(d) { return xStartLoc + x(d.WeightDJ)})
      .attr("cy", function(d, i) { return y(i) + yStartLoc; })
      // .style("fill", function(d) { return color(d.Name); });
      .style("fill", "blue")
      .style("cursor", "pointer")
      .on('mouseover', function(d){
          var xPosition = d3.event.pageX + 5;
          var yPosition = d3.event.pageY + 5;
          d3.select("#tooltip")
            .style("left", xPosition + "px")
            .style("top", yPosition + "px");
          d3.select("#tooltip")
            .text("Weight by Price: " + d.WeightDJ + "%");
          d3.select("#tooltip").classed("hidden", false);
      })
      .on('mouseout', function() {
        d3.select("#tooltip").classed("hidden", true);
      });

  groups.append("circle")// set up x,y coordinates of WeightMC - weight by marketcap
      .attr("class", "dot")
      .attr("r", 3.5)
      .attr("cx", function(d) { return xStartLoc + x(d.WeightMC)})
      .attr("cy", function(d, i) { return y(i) + yStartLoc; })
      .style("fill", "red")
      .style("cursor", "pointer")
      .on('mouseover', function(d){
          var xPosition = d3.event.pageX + 5;
          var yPosition = d3.event.pageY + 5;
          d3.select("#tooltip")
            .style("left", xPosition + "px")
            .style("top", yPosition + "px");
          d3.select("#tooltip")
            .text("Weight by MarketCap: " + d.WeightMC + "%");
          d3.select("#tooltip").classed("hidden", false);
      })
      .on('mouseout', function() {
        d3.select("#tooltip").classed("hidden", true);
      });

  groups.append("line") // add line for company records
      .attr("class", "company")
      .attr('x1', xStartLoc)
      .attr('y1', function (d,i) { return y(i) + yStartLoc; })
      .attr('x2', width)
      .attr('y2', function (d,i) { return y(i) + yStartLoc; })
      .attr("stroke-width", 1)
      .attr("stroke", "lightgrey");

      groups.append("text") //add company names along y-axis
          .attr("class", "label")
          .attr("y", function (d,i) { return y(i) + yStartLoc; })
          .attr("x", xStartLoc - 5)
          .text(function(d, i) { return (d.Name)})
          .style("text-anchor", "end")
          .style("cursor", "pointer")
          .style("fill", "#000")
          .on('mouseover', function(d){
              var xPosition = d3.event.pageX + 5;
              var yPosition = d3.event.pageY + 5;
              d3.select("#tooltip")
                .style("left", xPosition + "px")
                .style("top", yPosition + "px");
              d3.select("#tooltip")
                .text("Sector: " + d.Sector);
              d3.select("#tooltip").classed("hidden", false);
          })
          .on('mouseout', function() {
            d3.select("#tooltip").classed("hidden", true);
          });

  plotDiv.append("line")// add line at 3.33% to show equal weights
          .attr("class", "threshold")
          .attr('x1', function(d) { return xStartLoc + x(3.33)})
          .attr('y1', 0)
          .attr('x2', function(d) { return xStartLoc + x(3.33)})
          .attr('y2', height + margin.top)
          .attr("stroke-width", 1)
          .attr("stroke", "green");

     plotDiv.append("g") //add x-axis and label
            .attr("transform", "translate(" + xStartLoc + "," + (height + margin.top) + ")")
            .call(d3.axisBottom(x)
                  .ticks(15));
        // text label for the x axis
        plotDiv.append("text")
            .attr("transform",
                  "translate(" + (width/2) + " ," +
                                 (height + margin.top - 3 ) + ")")
            .style("text-anchor", "middle")
            .text("% of Index");

// add legend   // move legend somewhere else
var weightColor = [
    {"Name" : "Equal Weighting", "Color" : "green"},
    {"Name" : "Current Weighting by Price", "Color"  : "blue"},
    {"Name" : "Proposed Weighting by Market Cap", "Color"  :  "red"}
]
  var legend = plotDiv.selectAll(".legend")
      .data(weightColor)
      .enter().append("g")
      .attr("class", "legend")
      .attr("transform", function(d, i) { return "translate(0," + (y(i)+153) + ")"; });
      // move legend somewhere else

  legend.append("rect")
      .attr("x", width - 150)
        .attr("y", 5)
      .attr("width", 10)
      .attr("height", 10)
      .style("fill", function(d,i) {return d.Color});

  legend.append("text")
      .attr("class", "label")
      .attr("x", width - 175)
      .attr("y", 9)
      .attr("dy", ".35em")
      .style("text-anchor", "end")
      .text(function(d,i) { return d.Name; });

});

// Treemap of sector weights
// With help from http://bl.ocks.org/ndobie/90ae9f1a5c7f88ad4929
// and https://bl.ocks.org/mbostock/4063582

var treeDiv = d3.select("#treeSVG"),
    width = +treeDiv.attr("width"),
    height = +treeDiv.attr("height");

var fader = function(color) { return d3.interpolateRgb(color, "#fff")(0.2); },
    color = d3.scaleOrdinal(d3.schemeCategory20.map(fader)),
    format = d3.format(",d");

// data.format = d3.format(",.2n");  where can I set number format to two decimals?

var treemap = d3.treemap()
    .tile(d3.treemapResquarify)
    .size([width - 50, height])
    .round(true)
    .paddingInner(1);


function sumByCount(d) { // current Weighting
    return d.WeightMC;
};
function sumBySize(d) { // marketcap weighting
  return d.WeightDJ;
};

d3.json("sectorweights3.json", function(error, data) {
  if (error) throw error;

  var root = d3.hierarchy(data)
      .eachBefore(function(d) { d.data.id = (d.parent ? d.parent.data.id + "." : "") + d.data.name; })
      .sum(sumBySize)
      .sort(function(a, b) { return b.height - a.height || b.value - a.value; });

  treemap(root);

  var cell = treeDiv.selectAll("g")
    .data(root.leaves())
    .enter().append("g")
    .attr("transform", function(d) { return "translate(" + d.x0 + "," + d.y0 + ")"; });

  cell.append("rect")
      .attr("id", function(d) { return d.data.id; })
      .attr("width", function(d) { return d.x1 - d.x0; })
      .attr("height", function(d) { return d.y1 - d.y0; })
      .attr("fill", function(d) { return color(d.parent.data.id); });

  cell.append("text")
      .attr("text", function(d) { return d.data.name ; })  // doesn't wrap text
    .selectAll("tspan")
      .data(function(d) { return d.data.name.split(/(?=[A-Z][^A-Z])/g); })
    .enter().append("tspan")
      .attr("x", 4)
      .attr("y", function(d, i) { return 13 + i * 10; })
      .text(function(d) { return d; });

      cell.append("title")
      .text(function(d) { return d.data.name + "\n" + "Weight by Price: " + d.data.WeightDJ + "%" + "\n" +
              "Weight by Market Cap: " + d.data.WeightMC + "%"; });

      d3.selectAll("input")
            .data([sumBySize, sumByCount], function(d) { return d ? d.name : this.value; })
            .on("change", changed);

        var timeout = d3.timeout(function() {
          d3.select("input[value=\"sumByCount\"]")
              .property("checked", true)
              .dispatch("change");
        }, 2000);

        function changed(sum) {
          timeout.stop();   // fix timeout and toggle on input only

          treemap(root.sum(sum));

          cell.transition()
              .duration(750)
              .attr("transform", function(d) { return "translate(" + d.x0 + "," + d.y0 + ")"; })
            .select("rect")
              .attr("width", function(d) { return d.x1 - d.x0; })
              .attr("height", function(d) { return d.y1 - d.y0; });
        };
// add legend to treemap
// should be able to invoke color scale instead of hardcoding colors

    var sectorColors = [
        {"Name" : "Industrials", "Color" : "rgb(76, 146, 195)"},
        {"Name" : "Consumer", "Color" : "rgb(190, 210, 237)"},
        {"Name" : "Technology", "Color" : "rgb(255, 153, 62)"},
        {"Name" : "Financial", "Color" : "rgb(255, 201, 147)"},
        {"Name" : "Healthcare", "Color" : "rgb(86, 179, 86)"},
        {"Name" : "Oil & Gas", "Color" : "rgb(173, 229, 161)"},
        {"Name" : "Materials", "Color" : "rgb(222, 82, 83)"},
        {"Name" : "Telecom", "Color" : "rgb(255, 173, 171)"},
    ];

  var legend = d3.select("#legendSVG").selectAll(".legend")
      .data(sectorColors)
      .enter().append("g")
      .attr("class", "legend")
      .attr("transform", function(d, i) { return "translate(" + ((i*120) + 5) + ",5)"; });
      // move legend somewhere else

  legend.append("rect")
      // .attr("x", width - 150)
      .attr("x", 5)
      .attr("width", 12)
      .attr("height", 12)
      .style("fill", function(d,i) {return d.Color});

  legend.append("text")
      .attr("class", "label")
      // .attr("x", width - 125)
       .attr("x", 20)
      .attr("y", 6)
      .attr("dy", ".35em")
      // .style("text-anchor", "end")
      .text(function(d,i) { return d.Name; });
  });



</script>
</body>
</html>
