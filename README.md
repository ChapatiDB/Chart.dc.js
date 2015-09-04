# Chart.dc.js

Chart.dc.js is an extension of Chart.js devoted to the integration of Crossfilter with Chart.js, with support for dc.js chart groups. 

The need for this grew out of a desire to make use of the clean, simple and unique chart implementations in Chart.js, but to use them with the same kind of behavior that dc.js has incorporated to d3js charts.

Additionally, Chart.dc.js makes it possible to add your chart to a dc.js chartgroup, so these charts can be used in conjunction with dc.js charts or other charts that can be registered to the dc.js chartRegistry.

> Note Chart.dc.js so far is just a small personal project, and there is no guarantee that it will support future versions of its dependencies (d3.js, crossfilter.js, dc.js, Chart.js).

The only Chart.js chart available for Chart.dc.js at present is the Polar Area chart. Please feel free to make pull requests and contribute to flesh out the project with more Chart.js charts. 

# How to Install #

There is no support for Bower or dependency injection with Chart.dc.js at this time, but adding Chart.dc.js is fairly simple with traditional javascript includes. 

1. Add your dependencies first, in the appropriate order
	
	Dependency | Include Example (your directory name/structure may vary)
	--- | ---
	d3js <http://d3js.org> | `<script src='src/d3.js'></script>` 
	crossfilter `<http://square.github.io/crossfilter/>` |  `<script src='src/crossfilter.js'></script>` 
	dc-js <http://dc-js.github.io/dc.js/> | `<script src='dc.js'></script>`
	Chart.js <http://www.chartjs.org/> | `<script src='Chart.js'></script>`
2. Add the dependency for the Chart.dc.js chart you want to include
	`<script src='Chart.dc.PolarArea.js'></script>`

# How To Use #

   Declare your polar area chart element in HTML same as you would any Chart.js chart element. For example:

	<canvas id="myChart" width="400" height="400"></canvas>

   In your javascript, you will need to initialize crossfilter and create a crossfilter dimension to support your chart: 

```javascript

   var xf = crossfilter([some crossfilter ready data set])
   var myDimension = xf.dimension(function(d) { 
      return d.myColumn; 
   });

```

   Now for the fun part. Chart.js extensions inherit from the Chart.Type prototype object function which only accepts a single argument for the data and another for the options object. the format for the data is an array of objects that specify the format for the chart elements that make up the chart. We need to follow a slightly different format in order to bake in the crossfilter functionality without messing around in the internal chart code. 
   
   So while we have the same signature of other Chart.js constructor methods, what do we change? Basically, the 'data' argument you pass should not be an array of objects but instead take the following format:

```javascript



data: {
	dimension: // the crossfilter dimension you wish to use for this chart
	colors: // an array of colors, in the order that the crossfilter groupings should be colored
	highlights: //an array of highlights that should correspond to the colors array
	labels: //an array of labels (optional) that should correspond to the crossfilter group names as they should appear in tooltips. If non are supplied, the keys of the crossfilter groups will be used
	chartGroup: // the dc-js chart group this chart should belong to. If none is supplied, this chart will belong to chart group 0. 
}


```
