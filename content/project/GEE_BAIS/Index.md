---
title: Google Earth Engine Based Approach for Irrigation Scheduling
date: 2023-10-26
tags:
  - GEE
  - Remote Sensing
  - JavaScript
  - Machine Learning
  - GIS

links:
- name: erth engine link
  url: https://code.earthengine.google.com/
url_pdf: "/Israel_Geber_Remote Sensing_Final_Project.pdf"
url_code: 'https://github.com/'

---

# Israel Gebre


### Google Earth Engine Based Approach for Irrigation Scheduling

### Introduction

<p style="text-align: justify;">
Google Earth Engine is a cloud-based platform that allows users to analyze, visualize, and process large amounts of geospatial data. In Ethiopia, a country with a rapidly expanding economy and a high reliance on agriculture for its GDP, precision agriculture techniques such as satellite-based monitoring of crop water stress are important for ensuring that crops receive timely irrigation and do not suffer from water stress. Ethiopia has a significant water resource, with an estimated 122 billion m³ of water and 2.6-2.65 billion m³ of groundwater potential. However, despite this potential, the total land under irrigation in the country is currently less than 12% of its irrigable land. By using Google Earth Engine to analyze remote sensing and GIS data, it is possible to detect crop water stress and suggest irrigation scheduling to improve agricultural productivity and water management in the country. This research aims to use Google Earth Engine to assess water stress and inform irrigation scheduling in Ethiopia.

<p style="text-align: justify;">
One approach to using Google Earth Engine for irrigation scheduling in Ethiopia is to analyze satellite imagery to detect crop water stress. This can be done by calculating the Normalized Difference Vegetation Index (NDVI), which is a measure of plant health based on the reflectance of visible and near-infrared light. High NDVI values typically indicate healthy, well-watered plants, while low NDVI values may indicate water stress. By analyzing NDVI over time, it is possible to detect changes in plant health that may be due to water stress and use this information to schedule irrigation.

<p style="text-align: justify;">
Another approach is to use Google Earth Engine to access and analyze weather data, such as precipitation and evapotranspiration, to determine how much water crops are likely to need. This information can be used to schedule irrigation based on the predicted water requirements of the crops.

<p style="text-align: justify;">
In addition to satellite imagery and weather data, Google Earth Engine can also be used to visualize and analyze other data relevant to irrigation scheduling in Ethiopia, such as soil moisture and irrigation infrastructure. This can help farmers and agricultural professionals make informed decisions about when and how much to irrigate their crops, improving agricultural productivity and water management in the country.

### Objective

<p style="text-align: justify;">
<p>The objective of the project is to use satellite data to analyze the water stress levels of crops in real-time. The aim is to improve irrigation practices and increase crop production and farmer income. The project will also help irrigation planners to efficiently manage irrigation resources and prevent water waste.
<img src="/Et_vec.png"
     alt="Sentinel-2"
     style="float: left; margin-right: 10px;" />


<p style="text-align: justify;">
<p>The data source for the project will be Sentinel-2 satellite data, which will be used to calculate the NDVI and NDWI indices. These indices will be combined to create a new index called the CWSI, which will be used to detect crop water stress conditions.</p>

<img src="/Et_vec_1.png"
     alt="Sentinel-2"
     style="float: left; margin-right: 10px;" />

The study area for the project will be the Abay Blue Nile Basin and Rift Valley Basin. These areas were selected because they have access to irrigation water and are used for crop production. The project will also make use of a dynamic 10m near-real-time (NRT) Land Use/Land Cover (LULC) dataset, which includes class probabilities and label information for nine classes.

### Methodology

<p style="text-align: justify;">
The aim of this project is to use satellite data to analyze crop water stress levels in a specific region and optimize irrigation practices to increase crop production and farmer income. To do this, the team will calculate the Normalized Difference Water Index (NDWI), the Normalized Difference Vegetation Index (NDVI), and the Crop Water Stress Index (CWSI). These indices provide information about the water stress levels, growth and health of crops, and the combination of NDVI and NDWI values, respectively. The study will be conducted on cropped areas with an NDVI value greater than 0.25, and non-agricultural areas will be excluded using a Land Use/Land Cover (LULC) mask. The ultimate goal is to use the information provided by the NDWI, NDVI, and CWSI indices to optimize irrigation practices and improve crop production and farmer income.

#### Data Collection

<p style="text-align: justify;">
The study used Sentinel-2 satellite data, which is a European Space Agency (ESA) mission that provides high-resolution images of Earth's land surfaces, with a spatial resolution of 10 meters for visible and near-infrared (NIR) bands and 20 meters for shortwave infrared (SWIR) bands. The data used in the study was collected from October 19th to December 22nd, 2022.

```javascript
// Define the start and end dates for data collection
var startDate = '2022-10-19';
var endDate = '2022-12-22';

// Load the Sentinel-2 data for the specified date range and region
var s2 = ee.ImageCollection('COPERNICUS/S2_SR')
  .filterDate(startDate, endDate)
  .filterBounds(region)
  .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 35)); 
```

#### Data Preprocessing

<p style="text-align: justify;">
The data collected was filtered to include only images with less than 35% cloud cover and only within the specified region. The filtered data was then mosaic to create a single image for the study period.

```javascript
// Create a mosaic image from the filtered Sentinel-2 data
var s2Image = ee.Image(s2.mosaic()).clip(region);
```

#### Index Calculation
<p style="text-align: justify;">
The Normalized Difference Water Index (NDWI) and Normalized Difference Vegetation Index (NDVI) were calculated from the mosaic image using the following formulas:
-NDWI = (Band 8 (NIR) - Band 11 (SWIR)) / (Band 8 (NIR) + Band 11 (SWIR))
-NDVI = (Band 8 (NIR) - Band 4 (R)) / (Band 8 (NIR) + Band 4 (R))

```javascript
// Calculate the NDWI using the formula: NDWI = (Band 8 (NIR) - Band 11 (SWIR)) / (Band 8 (NIR) + Band 11 (SWIR))
var ndwi = s2Image.normalizedDifference(['B8', 'B11']).rename('NDWI');

// Calculate the NDVI using the formula: NDVI = (Band 8 (NIR) - Band 4 (R)) / (Band 8 (NIR) + Band 4 (R))
var ndvi = s2Image.normalizedDifference(['B8', 'B4']).rename('NDVI');

// Calculate the CWSI using the formula: CWSI = NDWI + NDVI
var cwsi = ndwi.add(ndvi);
```

#### Crop Water Stress Classification

<p style="text-align: justify;">
The CWSI values were classified into five categories based on surface temperature, vegetation density, and water content:

- Class 1: Normal condition, with no water stress and dense vegetation. Normal (CWSI > 1)
- Class 2: Low water stress condition, with slightly low surface temperature and sparse vegetation. Low (CWSI > 0.8 & < 1)
- Class 3: Moderate water stress condition, with sparse vegetation and low water content. Moderate (CWSI < 0.8 & > 0.5)
- Class 4: High water stress condition, with high surface temperature and very low water content and sparse vegetation. High (CWSI < 0.5 & > 0.35)
- Class 5: Severe water stress condition, with high surface temperature and very low water content and very low density of vegetation. Severe (CWSI < 0.35)

// Classify the CWSI values into five categories based on the defined thresholds
var cwsiClassification = cwsi.expression(
  '(cwsi > 1) ? 1 : (cwsi > 0.8) ? 2 : (cwsi > 0.5) ? 3 : (cwsi > 0.35) ? 4 : 5',
  { 'cwsi': cwsi }
);

```javascript 
// Classify the CWSI values into five categories based on the defined thresholds
var cwsiClassification = cwsi.expression(
  '(cwsi > 1) ? 1 : (cwsi > 0.8) ? 2 : (cwsi > 0.5) ? 3 : (cwsi > 0.35) ? 4 : 5',
  { 'cwsi': cwsi }
);
```
####  Cropped Area Extraction

<p style="text-align: justify;">
To focus on crop water stress conditions, the study was conducted only on cropped areas, with non-agricultural areas masked out using a Land Use and Land Cover (LULC) mask. The study was also limited to areas where the NDVI was greater than 0.25.

```javascript
// Load the LULC mask and apply it to the CWSI classification image
var lulc = ee.Image('USGS/NLCD/NLCD2011').clip(region);

var cwsiClassificationCropped = cwsiClassification.updateMask(lulc.eq(1));

// Filter the CWSI classification image to include only areas with an NDVI greater than 0.25
var cwsiClassificationCroppedNDVI = cwsiClassificationCropped.updateMask(ndvi.gt(0.25));
```


### Results & Discussions

<p style="text-align: justify;">
The results of this study demonstrate the effectiveness of using a crop water stress index (CWSI) for irrigation scheduling in the study area. The CWSI was determined by evaluating the water stress conditions of crops and grouping the output into five classes, as well as integrating two vegetation indices (NDVI and NDWI). Real-time analysis was conducted using the Google Earth Engine Code Editor platform, and a CWSI app was developed to assist with the proper selection of times for irrigation.

<img src="/Et_vec_2.png"
     alt="Sentinel-2"
     style="float: left; margin-right: 10px;" />

<p style="text-align: justify;">
The app allows users to select a start and end date to get a full image of the Abay Blue Nile Basin and Rift Valley Basin for CWSI. The results show that the CWSI can be used to determine the appropriate timing for irrigation, with no water stress requiring no irrigation, low water stress requiring some irrigation, moderate water stress indicating a need for irrigation, and severe water stress indicating an immediate need for irrigation.

<p style="text-align: justify;">
The findings suggest that the CWSI can be an effective tool for managing irrigation schedules and improving crop yield. It can help farmers make data-driven decisions about when to irrigate, thereby optimizing water use and reducing wastage. The integration of NDVI and NDWI indices allows for a more comprehensive assessment of crop water stress, making the CWSI a valuable tool for precision agriculture.

### Conclusion

<p style="text-align: justify;">
The use of a Crop Water Stress Index (CWSI) for irrigation scheduling in the study area has shown to be effective in determining the water stress conditions of crops. By integrating two vegetation indices (NDVI and NDWI), the CWSI was able to provide a more comprehensive assessment of crop water stress and aid in making data-driven decisions for irrigation scheduling. The development of the CWSI app allows for real-time analysis and proper selection of irrigation times, which can ultimately improve crop yield and optimize water usage. The results of this study suggest that the CWSI can be a valuable tool for precision agriculture and improving irrigation practices in the study area.

### References

- Erkossa, Teklu, Awulachew, Seleshi Bekele, Haileslassie, A., & Yilma, Aster Denekew. (2009). *Impacts of improving water management of smallholder agriculture in the Upper Blue Nile Basin*.

- Anda, A., Simon-Gáspár, B., & Soós, G. (2021). The application of a self-organizing model for the estimation of Crop Water Stress Index (CWSI) in soybean with different watering levels. *Water*, 13(22), 3306. [https://doi.org/10.3390/w13223306](https://doi.org/10.3390/w13223306)

- Eshetae, M. A., Hailu, B. T., & Demissew, S. (2019). Spatial characterization and distribution modelling of ensete ventricosum (wild and cultivated) in Ethiopia. *Geocarto International*, 36(1), 60–75. [https://doi.org/10.1080/10106049.2019.1588392](https://doi.org/10.1080/10106049.2019.1588392)

- Gontia, N. K., & Tiwari, K. N. (2008). Development of crop water stress index of wheat crop for scheduling irrigation using infrared thermometry. *Agricultural Water Management*, 95(10), 1144–1152. [https://doi.org/10.1016/j.agwat.2008.04.017](https://doi.org/10.1016/j.agwat.2008.04.017)

- Khomarudin, M. R., & Sofan, P. (2010). Crop water stress index (CWSI) estimation using Modis Data. *International Journal of Remote Sensing and Earth Sciences (IJReSES)*, 3(-). [https://doi.org/10.30536/j.ijreses.2006.v3.a1208](https://doi.org/10.30536/j.ijreses.2006.v3.a1208)

- Singh, P., Singh, A., & Kumar Upadhyay, R. (2021). A web based Google Earth engine approach for irrigation scheduling in Uttar Pradesh India using Crop Water Stress Index. *American Journal of Remote Sensing*, 9(1), 42. [https://doi.org/10.11648/j.ajrs.20210901.15](https://doi.org/10.11648/j.ajrs.20210901.15)



