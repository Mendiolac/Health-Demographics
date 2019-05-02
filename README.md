# Welcome! 



## Background

The data set included is based on 2014 ACS 1-year estimates: [https://factfinder.census.gov/faces/nav/jsf/pages/searchresults.xhtml](https://factfinder.census.gov/faces/nav/jsf/pages/searchresults.xhtml). The current data set includes the rates of income, obesity, poverty, etc. by state. MOE stands for "margin of error."

## Tools and languages used
 
 ### Tools
 
 Visual Studio Code and GitHub Pages
 
 ### Languages
 
 Javascript, Ploty.js, HTML, CSS, and SQL Database

## The Beginning

Created a `width` variable set to select a scatter plot in order to parse parse the data through as integers.
```
var width = parseInt(d3.select('#scatter')
.style("width"));

var height = width * 2/3;
var margin = 10;
var labelArea = 110;
var padding = 45;

```


## X and Y axis

Used D3 techniques to create an interactive scatter plot that allows the user to compare multiple demographics. First a variable is set to select the scatter plot and the different attributes are appended to create a SVG object.

```
var svg = d3.select("#scatter")
.append("svg")
.attr("width", width)
.attr("height", height)
.attr("class", "chart");

```

In order to create interchangeable x and y axis labels, a transform attribute is added. Each label is appended to the x-axis and then repeated for the y-axis. 

```
var bottomTextX =  (width - labelArea)/2 + labelArea;
var bottomTextY = height - margin - padding;
xText.attr("transform",`translate(
${bottomTextX}, 
${bottomTextY})`
);

```

Max and Min functions are created for scaling.
  
```
    function xMinMax() {
  xMin = d3.min(csvData, function(d) {
    return parseFloat(d[currentX]) * 0.85;
  });
  xMax = d3.max(csvData, function(d) {
    return parseFloat(d[currentX]) * 1.15;
  });     
}
```

The scales are determined by the previous functions.

```
var xScale = d3 
    .scaleLinear()
    .domain([xMin, xMax])
    .range([margin + labelArea, width - margin])

var yScale = d3
    .scaleLinear()
    .domain([yMin, yMax])
    .range([height - margin - labelArea, margin])
 ```
    
 X and Y axis utilize the scales and executed in a `tickCount()` function to transform between 5 and 10 ticks using an if/else statement depending on `width <= 530`. The axis are then appended to the SVG as group elements. 


## Tooltip and Circles

The circle radius is defined by the width of the scatter plot and uniform throughout the plot.

```
var cRadius;
function adjustRadius() {
if (width <= 530) {
cRadius = 7;}
else { 
cRadius = 10;}
}
adjustRadius();

```
The circles are appended for each row of data, the x and y scales configures the pixels.

```
allCircles.append("circle")
    .attr("cx", function(d) {
        return xScale(d[currentX]);
    })
    .attr("cy", function(d) {
        return yScale(d[currentY]);
    })
    .attr("r", cRadius)
    .attr("class", function(d) {
        return "stateCircle " + d.abbr;
    })
```

Tooltip is implimented to reveal a text box that will contain the current demographic percentages being reviewed. The text box is revealed on `mouseover` and `mouseout` functions. These functions also include a highlighted circle border in order to increase User Experience.

```
var toolTip = d3.tip()
  .attr("class", "d3-tip")
  .offset([40, -60])
  .html(function(d) {
        //Build text box
        var stateLine = `<div>${d.state}</div>`;
        var yLine = `<div>${currentY}: ${d[currentY]}%</div>`;
        if (currentX === "poverty") {
            xLine = `<div>${currentX}: ${d[currentX]}%</div>`}          
        else {
            xLine = `<div>${currentX}: ${parseFloat(d[currentX]).toLocaleString("en")}</div>`;}             
        return stateLine + xLine + yLine  
    });
    
  ```
    
 A `labelUpdate` function is created to update the current axis and switch between active and inactive.
    
```
function  labelUpdate(axis, clickText) {
    d3.selectAll(".aText")
        .filter("." + axis)
        .filter(".active")
        .classed("active", false)
        .classed("inactive", true);
        
    clickText.classed("inactive", false).classed("active", true);
    }
```
