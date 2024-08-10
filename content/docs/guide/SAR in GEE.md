---
title: SAR Data in Google Earth Engine 
weight: 5
---

# Working with SAR Data in Google Earth Engine

### Objective
The objective of this lab is to deepen your understanding of Synthetic Aperture Radar (SAR) data and learn how to visualize different composites in Google Earth Engine.

### Visualizing Sentinel-1 Data
1. Open up Earth Engine and type "Sentinel-1" into the search bar. Click on the Sentinel-1 result and read through the background information on the satellite and image properties.

    <img src="\image\SAR/1.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />
    
    <img src="\image\SAR/2.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

2. **Polarization Options**: 
   - "VV" means vertically polarized signal transmitted out and vertically polarized signal received.
   - "VH" refers to vertically polarized signal transmitted out, and horizontally polarized signal received.

3. **Filtering the Sentinel-1 Image Collection**:
   - Use the geometry tool to create a point geometry over your region of interest (we will use the Tully region of north Queensland, Australia, as an example) and rename it "roi".

    <img src="\image\SAR/3.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />


    ```javascript
    // Filter the collection for the VV product from the descending track
    var collectionVV = ee.ImageCollection('COPERNICUS/S1_GRD')
        .filter(ee.Filter.eq('instrumentMode', 'IW'))
        .filter(ee.Filter.listContains('transmitterReceiverPolarisation', 'VV'))
        .filter(ee.Filter.eq('orbitProperties_pass', 'DESCENDING'))
        .filterBounds(roi)
        .select(['VV']);
    print(collectionVV);

    // Filter the collection for the VH product from the descending track
    var collectionVH = ee.ImageCollection('COPERNICUS/S1_GRD')
        .filter(ee.Filter.eq('instrumentMode', 'IW'))
        .filter(ee.Filter.listContains('transmitterReceiverPolarisation', 'VH'))
        .filter(ee.Filter.eq('orbitProperties_pass', 'DESCENDING'))
        .filterBounds(roi)
        .select(['VH']);
    print(collectionVH);
    ```

4. **Console Information**:
   - Navigate to the console and have a look at the information you printed. Use the drop-down arrows to assess how many images are present in the collection for your region of interest.

    <img src="\image\SAR/4.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

5. **Centering the Map View**:

    ```javascript
    // Let's center the map view over our ROI
    Map.centerObject(roi, 13);
    ```

    <img src="\image\SAR/5.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

6. **Using the Median Reducer**:
   - Obtain the median pixel value across all years for each pixel.

    ```javascript
    var VV = collectionVV.median();
    ```

7. **Plotting the Median Pixel Values**:
   - Adjust the min and max visualization parameters according to your chosen scene. Use the inspectors to help you establish the value range.

    ```javascript
    // Adding the VV layer to the map
    Map.addLayer(VV, {min: -14, max: -7}, 'VV');
    ```

    <img src="\image\SAR/6.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

### Exploring the Image
   - Examine which landscape features have high backscatter intensity (white), and which have low intensity (black).

1. **Deriving the VH Median Layer and Mapping It**:

    ```javascript
    // Calculate the VH layer and add it
    var VH = collectionVH.median();
    Map.addLayer(VH, {min: -20, max: -7}, 'VH');
    ```

    <img src="\image\SAR/7.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />

2. **Comparing VV and VH**:
    - Explore how VV and VH differ in their sensitivity to different land surfaces.

3. **Creating an RGB Composite from the SAR Data**:
    - Create three layers that can be placed into the Red, Green, and Blue channels.

    ```javascript
    // Create a 3-band stack by selecting from different periods (months)
    var VV1 = ee.Image(collectionVV.filterDate('2018-01-01', '2018-04-30').median());
    var VV2 = ee.Image(collectionVV.filterDate('2018-05-01', '2018-08-31').median());
    var VV3 = ee.Image(collectionVV.filterDate('2018-09-01', '2018-12-31').median());

    // Add to map
    Map.addLayer(VV1.addBands(VV2).addBands(VV3), {min: -12, max: -7}, 'Season composite');
    ```

    <img src="\image\SAR/8.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />
---