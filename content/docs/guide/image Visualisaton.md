---
title: Image Visualisations 
weight: 1
sidebar:
  open: true
---
## Understanding Band Combinations

### Objective

<p style="text-align: justify;">
The objective of this lab is to strengthen your understanding of image visualization principles and develop practical skills in mapping band combinations and exploring reflectance properties of surface elements.

### Loading a Sentinel-2 Multispectral Image

<p style="text-align: justify;">
For this lab, we will use a multi-spectral image collected by the European Space Agency's Sentinel-2 satellite. Sentinel-2 is a wide-swath, high-resolution, multi-spectral imaging mission supporting Copernicus Land Monitoring studies, including the monitoring of vegetation, soil, and water cover, as well as observation of inland waterways and coastal areas. We will use an image collected over Kakadu National Park, Australia.

<p style="text-align: justify;">
Let's navigate to the area of interest (Kakadu) by copying the code below into the Code Editor and clicking "Run". Remember that the line starting with `//` is a note to ourselves and to others, and is not processed (we call this a comment). The numbers in brackets are the longitude, latitude, and zoom level (range is from 1 to 22).

```javascript
// Navigate to area of interest
Map.setCenter(132.5685, -12.6312, 8);
```

<img src="\image\image visualisations/1.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

**Figure 1.** Navigate to Kakadu

<p style="text-align: justify;">
Now that we are in the right place, let's choose a Sentinel-2 image using the code below. Copy and paste into the Code Editor and click "Run". Copernicus refers to the satellite mission, S2 is short for Sentinel-2, and the long number `20180422T012719_20180422T012714_T52LHM` refers to a specific image, defined by a date, time, and a path and row of the satellite's orbit. I have chosen a single image for the purposes of this lab, but we will cover searching for images for specific areas and dates at a later stage.

```javascript
// Select a specific Sentinel-2 image from the archive
var sent2 = ee.Image("COPERNICUS/S2/20180422T012719_20180422T012714_T52LHM");
```
<p style="text-align: justify;">
If the code did not return any errors, then the image was successfully found in the archive. To double-check, let's run the line below to print the image information to the Console. Once the information loads in the Console, you can click the little dropdown arrows next to "Image" and "bands" to see more details about the band structure and naming format.

```javascript
// Print image details to the Console
print(sent2);
```


<img src="\image\image visualisations/2.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

**Figure 2.** View image properties in Console

<p style="text-align: justify;">
We can see from the Console information that the image contains multiple bands, called B1, B2, B3, etc. To find out which wavelengths these bands represent, let's use the search bar to find out more information. Type "Sentinel-2" into the search bar and you will see it appear in the results list.


<img src="\image\image visualisations/3.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

**Figure 3.** Search for Sentinel-2

<p style="text-align: justify;">
Click on "Sentinel-2 MSI: MultiSpectral Instrument, Level 1-C" to open the information panel. The table provided is very useful for gaining a quick overview of the available bands, their wavelengths, and spatial resolutions.

<img src="\image\image visualisations/4.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

**Figure 4.** Overview of band information

<p style="text-align: justify;">
Now before we go any further, please save your current script by clicking the dropdown on the Save button, and selecting "Save as". Save it into your course repository so that you can come back to it at any stage, and from any device with a web browser.

<img src="\image\image visualisations/5.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

**Figure 5.** Save your script

### Working with Band Combinations

<p style="text-align: justify;">
Getting back to our image, Bands 2, 3, and 4 are the blue, green, and red bands respectively. Therefore, if we wish to view a true-color rendering of the image—i.e., an RGB composite—we need to place Band 4 into the red channel, Band 3 into the green channel, and Band 2 into the blue channel. We can do this with the code below—take careful note of the syntax for specifying the band arrangement.

```javascript
Map.addLayer(sent2, {bands:['B4','B3','B2']});
```

<img src="\image\image visualisations/6.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

**Figure 6.** First RGB

<p style="text-align: justify;">
After running the previous line of code, we can see that an image loads in the map viewer, but it is completely dark. This is because we did not specify any visualization parameters. Reflectance values for Sentinel-2 products range from 0 to 3000, so let's specify this in our code as shown below (noting that all visualization parameters are inside the `{}` brackets):

```javascript
Map.addLayer(sent2, {bands:['B4','B3','B2'], min:0, max:3000});
```

<img src="\image\image visualisations/7.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

**Figure 7.** Second RGB

<p style="text-align: justify;">
That looks better. This is a view similar to what we would see looking out of the window of an airplane, which is why we call it a true-color composite. All three of the bands used in creating this composite occur in the visible portion of the electromagnetic spectrum.

<p style="text-align: justify;">
Zoom in a bit closer using the wheel of your mouse. These images are a fantastic resource for environmental mapping and monitoring. The visible spectrum bands are at 10m spatial resolution, and the revisit time of the satellite constellation is every 6 days in this region. Thanks, ESA!


<img src="\image\image visualisations/8.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

**Figure 8.** Zoomed RGB

<p style="text-align: justify;">
Before we go any further, let's clean up our code a bit. We didn't comment on the last two lines—let's fix that, and let's give titles to the layers in the map view so that we know which is which in the layer tab. We can paste these lines over the previous two.

```javascript
// Add RGB composite to map, without parameters defined
Map.addLayer(sent2, {bands:['B4','B3','B2']}, "Black");

// Add RGB composite to map, with parameters defined
Map.addLayer(sent2, {bands:['B4','B3','B2'], min:0, max:3000}, "True-colour");
```

<img src="\image\image visualisations/9.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

**Figure 9.** Layer titles

### Creating a False-Color Composite

<p style="text-align: justify;">
If we look back to the table of Sentinel-2 wavelengths, we can see that Band 8 is in the NIR (near-infrared) spectrum. Therefore, to map a false-color composite, we need to put Band 8 into the red channel, move Band 4 into the green channel, and move Band 3 into the blue channel. The resulting image now shows photosynthetically active vegetation in vibrant red.

```javascript
// Add RGB composite to map, using NIR for false-color
Map.addLayer(sent2, {bands:['B8','B4','B3'], min:0, max:3000}, "False-colour");
```

<img src="\image\image visualisations/10.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

**Figure 10.** False-color composite

<p style="text-align: justify;">
Now you can navigate around the scene and flip between the true-color and false-color views using the layers tab. Take careful note of how different parts of the scene are represented in these different visualizations, and explore how some features, like burn scars, jump out more clearly in the false-color composite.

<img src="\image\image visualisations/11.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

**Figure 11.** Flip between layers

### Exploring Individual Bands

<p style="text-align: justify;">
To really build your understanding of how different wavelengths interact with surfaces, we are now going to load individual bands sequentially, from shorter to longer wavelengths. To display Band 1, the code is as follows:

```javascript
// Add Band 1 to map
Map.addLayer(sent2, {bands:['B1'], min:0, max:3000}, "B1");
```

<img src="\image\image visualisations/12.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

**Figure 12.** Display Band 1

To display more bands individually, the code is the same:

```javascript
// Add a few more bands to map
Map.addLayer(sent2, {bands:['B4'], min:0, max:3000}, "B4");
Map.addLayer(sent2, {bands:['B8'], min:0, max:3000}, "B8");
Map.addLayer(sent2, {bands:['B12'], min:0, max:3000}, "B12");
```

<p style="text-align: justify;">
Use the layers tab to turn bands off and on in the map display view. Take note of which landscape elements appear brighter and darker as you change band numbers (and therefore move from shorter to longer wavelengths).

### Practical Exercise

<p style="text-align: justify;">
Load up another Sentinel-2 image for the same area of interest. The code below contains the image identifier for an image collected in August 2018.

```javascript
// Select a specific Sentinel-2 image from the archive
var sent2dry = ee.Image("COPERNICUS/S2/20180810T012709_20180810T012711_T52LHM");
```

Visualize this image in true-color and false-color.

Compare the August image with the one from April. What has changed and why?

### Additional Band Combinations to Explore

<p style="text-align: justify;">
So far, we have only explored two visualization options (true-color and false-color), but there are many more possible RGB combinations:

- **Natural color:** 4 3 2
- **False color infrared:** 8 4 3
- **False color urban:** 12 11 4
- **Agriculture:** 11 8 2
- **Atmospheric penetration:** 12 11 8A
- **Healthy vegetation:** 8 11 2
- **Land/Water:** 8 11 4
- **Natural colors with atmospheric removal:** 12 8 3
- **Shortwave infrared:** 12 8 4
- **Vegetation analysis:** 11 8 4

Experiment with the combinations listed above and think about why we might want to use them.

### Exploring an Urban Setting
Use this image (COPERNICUS/S2/20180130T000241_20180130T000235_T56HLH) to explore a more urban setting in Sydney.


{{< cards >}}
  {{< card url="project-structure" title="Project Structure" icon="document-duplicate" >}}
  {{< card url="configuration" title="Configuration" icon="adjustments-vertical" >}}
{{< /cards >}}
