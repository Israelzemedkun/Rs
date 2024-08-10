---
title: Plotting Spectral Response Curves
weight: 6
---
### Objective
The objective of this lab is to further your understanding of spectral responses and develop skills in using the Charting functions in Earth Engine (JavaScript).

### Load Up a Landsat-8 Scene
1. Navigate to an area of interest for you.
2. Place a point marker on the map and rename it `roi`.
3. Run the code below to pull up a cloud-free image for a specific date range (adjust as needed):

```javascript
// Filter image collection for time window, spatial location, and cloud cover
var image = ee.Image(ee.ImageCollection('LANDSAT/LC08/C01/T1_SR')
    .filterBounds(roi)
    .filterDate('2016-05-01', '2016-06-30')
    .sort('CLOUD_COVER')
    .first());

// Add true-color composite to map
Map.addLayer(image, {bands: ['B4', 'B3', 'B2'], min: 0, max: 3000}, 'True colour image');
```

### Create Polygons for Different Classes
First, specify which bands to use and create new polygons using the rectangle tool for three classes (`Water`, `Forest`, `City`) that we want to explore.

  <img src="\image\Plotting SPC/1.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

**Figure 1.** Make rectangle polygons

Change the geometry type to `Feature` and define a `label` in the properties tab.

  <img src="\image\Plotting SPC/2.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />


**Figure 2.** Defining Feature and Labels

```javascript
// Choose bands to include and define feature collection to use
var subset = image.select('B[1-7]');
var samples = ee.FeatureCollection([Water, Forest, City]);
```

### Create and Print a Chart
Now we can create a chart variable and print it to the console. We use the `image.regions` function to summarize by class region, and the `ee.Reducer.mean()` function to obtain the mean reflectance value for each class for each band.

```javascript
// Create the scatter chart
var Chart1 = ui.Chart.image.regions(
    subset, samples, ee.Reducer.mean(), 10, 'label')
        .setChartType('ScatterChart');
print(Chart1);
```
  <img src="\image\Plotting SPC/3.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

**Figure 3.** Chart 1

### Customize the Chart
We can improve the readability of our chart by specifying some display options:

```javascript
// Define customization options.
var plotOptions = {
  title: 'Landsat-8 Surface reflectance spectra',
  hAxis: {title: 'Wavelength (nanometers)'},
  vAxis: {title: 'Reflectance'},
  lineWidth: 1,
  pointSize: 4,
  series: {
    0: {color: 'blue'}, // Water
    1: {color: 'green'}, // Forest
    2: {color: 'red'}, // City
}};
```

We can print the actual band wavelengths on the x-axis using this:

```javascript
// Define a list of Landsat-8 wavelengths for X-axis labels.
var wavelengths = [443, 482, 562, 655, 865, 1609, 2201];
```

Finally, create the chart with more options and display it:

```javascript
// Create the chart and set options.
var Chart2 = ui.Chart.image.regions(
    subset, samples, ee.Reducer.mean(), 10, 'label', wavelengths)
    .setChartType('ScatterChart')
    .setOptions(plotOptions);

// Display the chart.
print(Chart2);
```

**Figure 4.** Chart 2
