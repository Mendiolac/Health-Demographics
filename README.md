# Welcome! 



## Background

The data set included is based on 2014 ACS 1-year estimates: [https://factfinder.census.gov/faces/nav/jsf/pages/searchresults.xhtml](https://factfinder.census.gov/faces/nav/jsf/pages/searchresults.xhtml). The current data set includes the rates of income, obesity, poverty, etc. by state. MOE stands for "margin of error."

## D3

### X and Y axis

Used D3 techniques to create an interactive scatter plot that allows the user to compare multiple demographics. First a variable is set to select the scatter plot and the different attributes are appended to create base dimensions.

```
var svg = d3.select("#scatter")
.append("svg")
.attr("width", width)
.attr("height", height)
.attr("class", "chart");

```

In order to create interchangeable x and y axis labels, a transform attribute is added.

```
var bottomTextX =  (width - labelArea)/2 + labelArea;
var bottomTextY = height - margin - padding;
xText.attr("transform",`translate(
${bottomTextX}, 
${bottomTextY})`
);

```

Each label is appended to the x-axis and then repeated for the y-axis. 

### Tooltip and Circles

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

Tooltip is implimented to reveal a text box that will reveal the current demographic percentages being reviewed. Tooltip will be used again in `mouseover' and 'mouseout' functions.

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
