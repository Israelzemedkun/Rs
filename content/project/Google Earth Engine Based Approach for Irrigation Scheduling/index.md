---
title: GEE
date: 2023-10-26
tags:
  - Hugo
  - Wowchemy
  - Markdown
---

# GIS Final Project

## The impact of COVID-19 lockdown on air quality
### Israel Gebre
### Final Project

---

### Introduction
The COVID-19 pandemic has had a significant impact on global health and economic systems. The World Health Organization labeled the outbreak a global pandemic on January 30, 2020, indicating the urgent need for action to stop the virus's spread. In reaction to the fast spread of COVID-19, several countries enacted lockdowns, which included the closure of non-essential enterprises as well as limitations on transportation and social gatherings. These safeguards were put in place to restrict the spread of the virus and reduce the strain on healthcare services. The lockdowns had substantial economic and social effects, including disruptions in the transportation and industrial sectors, as well as a reduction in worldwide fossil fuel consumption.

In India, the first case of COVID-19 was reported in the state of Kerala on January 30, 2020. In response to the growing threat of the virus, the Indian government implemented a nationwide lockdown on March 22, 2020, which was subsequently extended for 21 days starting on March 24, 2020. During the lockdown, all public gathering places in the country were closed, providing a unique opportunity to generate baseline data on air quality during a period of minimal anthropogenic activity.

To understand the impact of the lockdown on air quality and inform future mitigation policies, it is necessary to conduct a quantitative assessment of air quality variables. The goal of this study is to assess the changes in ambient air quality that occurred during the lockdown and subsequent unlock phases in India. To achieve this, I will analyze data on air pollutant concentrations and meteorological parameters using machine learning techniques. Data sources for this analysis will include Terra & Aqua MAIAC Land Aerosol Optical Depth (MODIS/006/MCD19A2_GRANULES) satellite data to evaluate the impact of fires on air quality. By identifying trends and patterns and understanding the complex relationships between emission sources, meteorology, and air quality, this research aims to provide valuable insights for the development of future air quality policies and actions in India.

---

### Prior Studies and Existing Literature
Several studies have examined the impact of the COVID-19 pandemic on air quality, with varying results. A study published in Environmental Research Letters found that the lockdowns implemented in China resulted in a significant decrease in air pollution, with reductions in PM2.5 concentrations ranging from 20-50%. Another study published in Nature Sustainability found that the lockdowns in India led to a reduction in particulate matter and nitrogen dioxide concentrations, as well as an increase in ozone concentrations.

On the other hand, a study published in Environmental Science & Technology found that the decrease in transportation and industrial activity during the pandemic led to a reduction in fine particulate matter concentrations in several cities in the United States, but that these reductions were not sustained once lockdowns were lifted. A study published in Environmental Research Letters also found that the decrease in air pollution during the pandemic led to an increase in life expectancy in some areas, but that this gain may be offset by the negative health impacts of the pandemic itself.

These studies highlight the complex and varied impacts of the COVID-19 pandemic on air quality, and the importance of conducting further research to understand the long-term effects and inform the developing effective strategies for addressing air quality issues.

---

### Methodology

#### Data Collection
This study involved using Google Earth Engine to analyze satellite data from the Terra and Aqua MAIAC Land Aerosol Optical Depth (MODIS/006/MCD19A2_GRANULES) dataset, covering the period from January 1, 2017, to May 3, 2020. The dataset provides daily global estimates of aerosol optical depth at a resolution of 10x10 km. It includes information on the concentration of various aerosols in the atmosphere and their ability to absorb or scatter light. In this study, I focused on the Optical_Depth_047 band, which specifically measures the concentration of fine particulate matter in the air.

```python
# Define the image collections for each year
var imgCollection2017 = ee.ImageCollection('MODIS/006/MCD19A2_GRANULES').select('Optical_Depth_047').filterDate('2017-01-01', '2017-03-24')

var imgCollection2017_lockdown = ee.ImageCollection('MODIS/006/MCD19A2_GRANULES').select('Optical_Depth_047').filterDate('2017-03-25', '2017-05-03')

var imgCollection2018 = ee.ImageCollection('MODIS/006/MCD19A2_GRANULES').select('Optical_Depth_047').filterDate('2018-01-01')
```

#### Data Processing and Analysis
I will analyze data from the years 2017 to 2020 to compare the air quality during lockdown periods to pre-lockdown periods in each year. The code first defines image collections for each year, separated by lockdown and pre-lockdown periods. These image collections are created using the "ee.ImageCollection" function and filtered by date using the "filterDate" method. The code then calculates the "mean" value for each image collection using the mean function.

```python
# Calculate the mean value for each image collection
var mean2017 = imgCollection2017.mean()
var mean2017_lockdown = imgCollection2017_lockdown.mean()
var mean2018 = imgCollection2018.mean()
var mean2018_lockdown = imgCollection2018_lockdown.mean()
var mean2019 = imgCollection2019.mean()
var mean2019_lockdown = imgCollection2019_lockdown.mean()
var mean2020 = imgCollection2020.mean()
var mean2020_lockdown = imgCollection2020_lockdown.mean()
```

Next, it subtracts the mean value of the pre-lockdown period from the mean value of the lockdown period to get the difference between the two periods.

```python
# Calculate the difference between the lockdown and pre-lockdown periods
var diff2017 = mean2017_lockdown.subtract(mean2017)
var diff2018 = mean2018_lockdown.subtract(mean2018)
var diff2019 = mean2019_lockdown.subtract(mean2019)
var diff2020 = mean2020_lockdown.subtract(mean2020)
```

To focus the analysis on a specific region of India and exclude areas outside of the region, I used the "clipToCollection" function to clip the image collections and difference maps to a region of interest, defined as a boundary shapefile imported as a table.

```python
# Add the difference maps to the map
Map.addLayer(diff2017.clipToCollection(region), band_viz, 'Difference 2017')
Map.addLayer(diff2018.clipToCollection(region), band_viz, 'Difference 2018')
Map.addLayer(diff2019.clipToCollection(region), band_viz, 'Difference 2019')
Map.addLayer(diff2020.clipToCollection(region), band_viz, 'Difference 2020')
```

#### Data Visualization
The resulting difference maps are visualized using the "Map.addLayer" function and the specified visualization parameters. The code also includes a section that creates lists of the image collections and mean values for each year, and uses these lists to create a chart showing the mean optical depth of aerosols during the lockdown and unlock phases. The chart is generated using the "ui.Chart.image.seriesByRegion" function, which takes a list of image collections and a region of interest as inputs, and returns a chart of the mean values for each region.

```python
# Create a list of the image collections to include in the chart
var imgCollections = [imgCollection2017, imgCollection2017_lockdown, imgCollection2018, imgCollection2018_lockdown, imgCollection2019, imgCollection2019_lockdown, imgCollection2020, imgCollection2020_lockdown]

# Create a list of labels for the image collections
var labels = ['2017', '2017 lockdown', '2018', '2018 lockdown', '2019', '2019 lockdown', '2020', '2020 lockdown']

# Calculate the mean values for each image collection
var means = imgCollections.map(function(imgCollection) { return imgCollection.mean();})

# Create the bar chart
var chart = ui.Chart.image.series(means, region, ee.Reducer.mean(), 500).setOptions({
    title: 'Mean AOD value',
    hAxis: {title: 'Year'},
    vAxis: {title: 'Mean AOD value'},
    series: {0: {color: 'blue'}, 1: {color: 'red'}, 2: {color: 'blue'}, 3: {color: 'red'}, 4: {color: 'blue'}, 5: {color: 'red'}, 6: {color: 'blue'}, 7: {color: 'red'}}
})

# Display the chart
print(chart)
```

This difference map is then used to visualize and analyze the changes in air quality during the lockdown periods. However, I were not able to visualize and analyze the changes in air quality during the lockdown periods because the Google Earth Engine had a memory limit and I exceeded this limit when trying to do the visualization and analysis.

For that, I wrote a code that is intended to export data from the image collection diff2017 for a series of regions within India. The regions are defined as polygons buffered around a central region of India by increasing distances. The process is repeated 30 times to create a list of 30 regions.

The image collection is first converted to a feature collection using the map function. This is done so that the image collection can be used with the clip