---
title: Introduction to Remote Sensing
date: 2024-02-17
weight: 1
---

### Introduction to Remote Sensing of the Environment

<img src="/skysat.webp" alt="AOD average" style="float: left; margin-right: 10px;" />

## Getting Started with Google Earth Engine

### Prerequisites
Completion of this lab exercise requires the use of the Google Chrome browser and a Google Earth Engine account. If you have not yet signed up, please do so now in a new tab: [Earth Engine account registration](https://earthengine.google.com/signup/). Once registered, you can access the Earth Engine environment here: [https://code.earthengine.google.com](https://code.earthengine.google.com)

Google Earth Engine uses the JavaScript programming language. We will cover the very basics of this language during this course. If you would like more detail, you can read through the introduction provided here: [JavaScript background](https://www.w3schools.com/js/)

### Objective
The objective of this lab is to introduce you to the Google Earth Engine processing environment. By the end of this exercise, you will be able to search, find, and visualize a broad range of remotely sensed datasets. We will start with single-band imagery - elevation data from the SRTM mission.

## 1. The Earth Engine Code Editor

<img src="/image-1.png" alt="Google Earth Engine environment" style="float: left; margin-right: 10px;" />

**Figure 1.** The Google Earth Engine environment

### Editor Panel
- The Editor Panel is where you write and edit your Javascript code.

### Right Panel
- **Console tab** for printing output.
- **Inspector tab** for querying map results.
- **Tasks tab** for managing long-running tasks.

### Left Panel
- **Scripts tab** for managing your programming scripts.
- **Docs tab** for accessing documentation of Earth Engine objects and methods, as well as a few specific to the Code Editor application.
- **Assets tab** for managing assets that you upload.

### Interactive Map
- For visualizing map layer output.

### Search Bar
- For finding datasets and places of interest.

### Help Menu
- **User guide** - reference documentation.
- **Help forum** - Google group for discussing Earth Engine.
- **Shortcuts** - Keyboard shortcuts for the Code Editor.
- **Feature Tour** - overview of the Code Editor.
- **Feedback** - for sending feedback on the Code Editor.
- **Suggest a dataset** - We appreciate suggestions on which new datasets we should ingest into the Earth Engine public archive.

## 2. Getting Started with Images
- Navigate to Darwin and zoom in using the mouse wheel.
  
<img src="/image-2.png" alt="Zoom to Darwin" style="float: left; margin-right: 10px;" />

- Clear the script workspace by selecting "Clear script" from the Reset button dropdown menu.

<img src="/image-3.png" alt="Zoom to Darwin" style="float: left; margin-right: 10px;" />

**Figure 3.** Clear script

- Search for “elevation” and click on the SRTM Digital Elevation Data 30m result to show the dataset description.

<img src="/image-4.png" alt="Zoom to Darwin" style="float: left; margin-right: 10px;" />

**Figure 4.** Search for elevation data

- View the information on the dataset, and then click on Import, which moves the variable to the Imports section at the top of your script.

<img src="/image-5.png" alt="Zoom to Darwin" style="float: left; margin-right: 10px;" />

**Figure 5.** View elevation datasource and import

- Rename the default variable name "image" to "srtm".

<img src="/image-6.png" alt="Zoom to Darwin" style="float: left; margin-right: 10px;" />

**Figure 6.** Rename image

- Add the image object to the console by copying the script below into the code editor, and click "run":
```javascript
print(srtm);
```
<img src="/image-7.png" alt="Zoom to Darwin" style="float: left; margin-right: 10px;" />

**Figure 7.** Print SRTM

<img src="/image-8.png" alt="Zoom to Darwin" style="float: left; margin-right: 10px;" />



- Browse through the information that was printed to the console. Open the “bands” section to show the one band named “elevation”. Note that all this same information is automatically available for all variables in the Imports section.

- Use the `Map.addLayer()` method to add the image to the interactive map. We will start simple, without using any of the optional parameters.

```javascript
Map.addLayer(srtm);
```

### Adjusting Visualization Parameters
The displayed map will look pretty flat grey because the default visualization parameters map the full 16-bit range of the data onto the black–white range, but the elevation range is much smaller than that in any particular location. We’ll fix it in a moment.

<img src="/image-9.png" alt="Zoom to Darwin" style="float: left; margin-right: 10px;" />

**Figure 8.** Map SRTM

- Select the Inspector tab. Then click on a few points on the map to get a feel for the elevation range in this area.

<img src="/image-10.png" alt="Zoom to Darwin" style="float: left; margin-right: 10px;" />

**Figure 9.** Inspect SRTM

Now you can set some more appropriate visualization parameters by adjusting the code as follows (units are in meters above sea level):

```javascript
Map.addLayer(srtm, {min: 0, max: 300});
```

<img src="/image-11.png" alt="Zoom to Darwin" style="float: left; margin-right: 10px;" />

**Figure 10.** Visualize SRTM

You will now be able to see variation in elevation range with low values in black and highest points in white. Layers added to the map will have default names like "Layer 1", "Layer 2", etc. To improve readability, we can give each layer a human-readable name, by adding a title with the syntax in the following code. Don't forget to click run.

```javascript
Map.addLayer(srtm, {min: 0, max: 300}, 'Elevation above sea level');
```

<img src="/image-12.png" alt="Zoom to Darwin" style="float: left; margin-right: 10px;" />

**Figure 11.** Rename title

### Commenting and Saving Your Scripts
Now, the last step for today is to save your code. However, before doing that, it is good practice to add some comment lines to your code reminding you of what you did and why. We add these with two forward slashes `//`:

```javascript
// Print data details to console
print(srtm);

// Add the SRTM data to the interactive map
Map.addLayer(srtm)

// Add the data again, but with restricted value ranges for better visualization
Map.addLayer(srtm, {min: 0, max: 300})

// Add the data again, with value ranges, and a useful title for the Layer tab
Map.addLayer(srtm, {min: 0, max: 300}, 'Elevation above sea level');
```

<img src="/image-13.png" alt="Zoom to Darwin" style="float: left; margin-right: 10px;" />

**Figure 12.** Comment script

The next step is then to save your script by clicking "Save". It will be saved in your private repository and will be accessible the next time you log in to Earth Engine.

<img src="/image-14.png" alt="Zoom to Darwin" style="float: left; margin-right: 10px;" />

**Figure 13.** Save script

If you would like to experiment with different color combinations, you can play with color palettes as per the example below:

```javascript
Map.addLayer(srtm, {min: 0, max: 300, palette: ['blue', 'yellow', 'red']}, 'Elevation above sea level');
```
<img src="/image-15.png" alt="Zoom to Darwin" style="float: left; margin-right: 10px;" />

**Figure 14.** Color scale elevation

### Hillshading and Slope
For better visualization, we can create a hillshade view of the elevation data. Remember you can use the Layer transparency options to create draped images for colorized hillshades.

```javascript
var hillshade = ee.Terrain.hillshade(srtm);
Map.addLayer(hillshade, {min: 150, max:255}, 'Hillshade');
```

<img src="/image-16.png" alt="Zoom to Darwin" style="float: left; margin-right: 10px;" />

**Figure 15.** Hillshade view

Slope works in a similar way:

```javascript
var slope = ee.Terrain.slope(srtm);
Map.addLayer(slope, {min: 0, max: 20}, 'Slope');
```
<img src="/image-17.png" alt="Zoom to Darwin" style="float: left; margin-right: 10px;" />

**Figure 16.** Slope map

Thank you.