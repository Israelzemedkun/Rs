---
title: Spectra to Indices, and Finding the Right Image
weight: 1
---
\image\Spectra to Indices

### Objective
The objective of this lab is to gain an understanding of a range of spectral indices and develop the skills for calculating any index you require. Before getting to that, we will build upon last week's lab and learn how to find an image for any geographic location of interest.

Just above the Coding panel is the search bar. Search for ‘Darwin’ in this GEE search bar, and click the result to pan and zoom the map to Darwin.

<img src="\image\Spectra to Indices/1.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

**Figure 1.** Navigating to the area of interest in Google Earth Engine

Use the geometry tools to make a point on Casuarina campus of Charles Darwin University (located in the suburb of Brinkin, north of Rapid Creek). Once you create the geometry point, you will see it added to your Coding panel as a variable (`var`) under the Imports heading.

<img src="\image\Spectra to Indices/2.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

**Figure 2.** Creating a geometry point

Rename the resulting point ‘campus’ by clicking the import name (which is called ‘geometry’ by default).

<img src="\image\Spectra to Indices/3.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

**Figure 3.** Renaming a geometry point

Search for ‘Sentinel-2’ in the search bar. In the results section, you will see ‘Sentinel-2: Multi-spectral Instrument (MSI), Level-1C’. Click on it and then click the ‘Import’ button.

<img src="\image\Spectra to Indices/4.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

**Figure 4.** Importing Sentinel-2 data

After clicking import, Sentinel-2 will be added to our Imports in the Coding panel as a variable. It will be listed below our campus geometry point with the default name `imageCollection`. Let's rename this to `sent2` by clicking on `imageCollection` and typing "sent2".

<img src="\image\Spectra to Indices/5.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

**Figure 5.** Renaming the imported data

It is important to understand that we have now added access to the full Sentinel-2 image collection (i.e., every image that has been collected to date) to our script. For this exercise, we don't want to load all these images—we want a single cloud-free image over Charles Darwin University. As such, we can now filter the image collection with a few criteria, such as time of acquisition, spatial location, and cloud cover.

### Filtering Image Collections
To achieve this, we need to use a bit of coding. In the JavaScript programming language, two backslashes (`//`) indicate comment lines and are ignored in actual processing steps. We use `//` to write notes to ourselves in our code so that we (and others who might want to use our code) can understand why we have done certain things.

```javascript
// This is our first line of code. Let’s define the image collection we are working with by writing this command
var image = ee.Image(sent2

// We will then include a filter to get only images in the date range we are interested in
.filterDate("2015-07-01", "2017-09-30")

// Next we include a geographic filter to narrow the search to images at the location of our point
.filterBounds(campus)

// Next we will also sort the collection by a metadata property, in our case cloud cover is a very useful one
.sort("CLOUD_COVERAGE_ASSESSMENT")

// Now let's select the first image out of this collection - i.e., the most cloud-free image in the date range
.first());

// And let's print the image to the console.
print("A Sentinel-2 scene:", image);
```
You need to copy the entire piece of code above and paste it in the “New script” box of the GEE code editor. Then click the "Run" button and watch Google do its magic... This piece of code will search the full Sentinel-2 archive, find images that are located over Darwin, sort them according to percentage cloud cover, and then return the most recent cloud-free image for us. Information relating to this image will be printed to the Console, where it is listed as "A Sentinel-2 scene" with some details about that scene (COPERNICUS/S2/20160629T014038_20160629T062926_T52LFM (16 bands)). We know from the scene name that it was collected on the 29th of June 2016.


<img src="\image\Spectra to Indices/6.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

**Figure 6.** Filtering the collection

### Adding Images to the Map View
Now, in order to actually have a look at this image, we need to add it to our mapping environment. Before doing that, however, let's define how we want to display the image. Let’s start with a true-color representation by pasting the following lines below the ones you’ve already added, and click "Run".

```javascript
// Define visualization parameters in a JavaScript dictionary for true-color rendering. Bands 4, 3, and 2 needed for RGB.
var trueColour = {
    bands: ["B4", "B3", "B2"],
    min: 0,
    max: 3000
};

// Add the image to the map, using the visualization parameters.
Map.addLayer(image, trueColour, "true-colour image");
```
This code specifies that for a true-color image, bands 4, 3, and 2 should be used in the RGB composite. After the image appears in the map, you can zoom in and explore Darwin. We see great detail in the Sentinel-2 image, which is at 10m resolution for the selected bands. The (+) and (-) symbols in the upper left corner of the map can be used for zooming in and out (also possible with the mouse scroll wheel/trackpad). A left-click with the mouse brings up the "hand" for panning to move around the image. Moving your mouse over the "Layers" button in the top right-hand corner of the map panel shows you the available layers and lets you adjust the opacity of different layers.

<img src="\image\Spectra to Indices/7.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

**Figure 7.** Adding a true-color image to the map

In order to find out more information at specific locations, we can use the Inspector tool, which is located in the Console Panel—left-hand tab. Click on the Inspector tab and then click on the image in the map view. Wherever you click on the image, the band values at that point will be displayed in the Inspector window. Click over some different patch types (sports fields, mangroves, ocean, beach, houses) to see how the spectral profile changes.

Now let's have a look at a false-color composite—we need to bring in the near-infrared band (band 8) for this. Paste the following lines below the ones you’ve already added, and click "Run".

```javascript
// Define false-color visualization parameters.
var falseColour = {
    bands: ["B8", "B4", "B3"],
    min: 0,
    max: 3000
};

// Add the image to the map, using the visualization parameters.
Map.addLayer(image, falseColour, "false-color composite");
```
<img src="\image\Spectra to Indices/8.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

**Figure 8.** Adding a false-color composite to the map

False-color composites place the near-infrared band in the red channel, and we see a strong response to the chlorophyll content in green leaves. Vegetation that appears dark green in true color appears bright red in the false color. Note the variations in red that can be seen in the vegetation bordering Rapid Creek. You will also see that "false-color composite" has been added to the Layers tab in the map view.

### Calculating NDVI
Next, let's calculate the normalized-difference vegetation index (NDVI) for this image. NDVI is an index calculated from the RED and NIR bands, according to this equation:
\[ \text{NDVI} = \frac{(\text{NIR} - \text{RED})}{(\text{NIR} + \text{RED})} \]

Paste the following lines below the ones you’ve already added, and click "Run". NDVI values range from 0 to 1, and the higher the value, the more "vigorous" the vegetation.

```javascript
// Define variable NDVI from the equation
var NDVI = image.expression(
    "(NIR - RED) / (NIR + RED)",
    {
        RED: image.select("B4"),    //  RED
        NIR: image.select("B8"),    // NIR
        BLUE: image.select("B2")    // BLUE
    }
);

Map.addLayer(NDVI, {min: 0, max: 1}, "NDVI");
```
<img src="\image\Spectra to Indices/9.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

**Figure 9.** Retrieving NDVI from Sentinel-2

Explore different parts of the image and see how NDVI values vary with different substrate types.
