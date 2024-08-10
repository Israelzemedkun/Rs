---
title: "Mapping Teak Tree Plantations"
authors:
- admin
date: "2024-08-07T00:00:00Z"
doi: ""

# Schedule page publish date (NOT publication's date).
publishDate: "2024-04-01T00:00:00Z"

# Publication type.
# Accepts a single type but formatted as a YAML list (for Hugo requirements).
# Enter a publication type from the CSL standard.
#publication_types: ["article"]

# Publication name and optional abbreviated publication name.
publication: ""
publication_short: ""

abstract: This study explores the use of remote sensing and time series analysis to map and monitor Teak Tree Plantations in Odisha, Madhya Pradesh, and Coimbatore. Utilizing Landsat 8 satellite imagery, the research employs spectral indices such as NDVI, EVI, and GNDVI to evaluate vegetation health over a four-year period from 2019 to 2023. Wavelet Transform techniques were applied to analyze temporal patterns in the data, revealing cyclical trends and filtering out high-frequency noise. The findings provide significant insights into the spatial and temporal variability of Teak plantations, highlighting their growth cycles and environmental responses. By integrating spatial smoothing, outlier removal, and advanced denoising methods, the study enhances the accuracy of vegetation assessments, offering a robust framework for sustainable forestry management and conservation.

# Summary. An optional shortened abstract.
summary: This study employs remote sensing and time series analysis to monitor Teak Tree Plantations across multiple regions in India using Landsat 8 imagery. Key spectral indices and wavelet transforms reveal significant temporal and spatial patterns in vegetation health from 2019 to 2023. The findings enhance understanding of Teak plantation dynamics, supporting sustainable forestry management.

tags:
- Remote Sensing
- Teak Tree Plantations
- Landsat 8
- Vegetation Monitoring
- Time Series Analysis
- Forestry Management
- Spectral Indices
- Wavelet Transforms

featured: true

links:
- name: Custom Link
  url: '#'
url_pdf: '#'
url_code: 'https://github.com/Israelzemedkun'
url_dataset: '#'
url_poster: '#'
url_project: ''
url_slides: ''
url_source: ''
url_video: ''

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder. 
#image:
#  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/s9CC2SKySJM)'
#  focal_point: ""
#  preview_only: false

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
projects:
- internal-project

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
slides: example
---

# Mapping Teak Tree Plantations

### Identifying/Mapping Teak Tree Plantations Using Landsat 8 Image Collection

**Objective**: The primary goal of this project is to analyze time series vegetation data using Wavelet Transform techniques. By leveraging the power of Landsat 8's satellite imagery, we aim to map Teak Tree Plantations and uncover underlying patterns that can provide insights into the health, distribution, and management of these plantations.

## Outline

- **Study Area**: Definition and geographic details of the regions being studied.
- **Methods**: An overview of the analytical techniques employed, including wavelet transforms and spectral indices.
- **Data Collection**: Details on the sources, types, and nature of the data used.
- **Data Preprocessing**: Steps taken to clean and prepare the data for analysis.
- **Results and Insights**: Key findings from the data analysis, including visualizations.
- **Conclusion**: Summary of the findings and their implications.
- **Way Forward**: Potential future directions for research and application.

### Dataset

The dataset includes vegetation indices derived from the Landsat 8 satellite's imagery, focusing specifically on the Enhanced Vegetation Index (EVI) and Normalized Difference Vegetation Index (NDVI) for selected regions within Madhya Pradesh, Odisha, and Coimbatore.

## Study Area

**Regions**: This study focuses on three key regions: Odisha, Madhya Pradesh, and Coimbatore. These areas are known for their significant Teak plantations, making them ideal for the study of vegetation dynamics using remote sensing data.

<img src="\preprint\study area.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />
<p style="text-align: justify;">

*Explanation*: This image represents the geographic boundaries of the study areas. By focusing on these regions, the study aims to provide a localized understanding of Teak plantation dynamics.

## Data Collection and Preparation

- **Samples**: Data samples were provided by collaborators, including Shekhar and Dr. Agarwala. These samples include both raw and processed satellite imagery specific to the study regions.
- **Images**: Satellite images were collected over a span of four years, from 2019 to 2023. This time frame allows for the analysis of seasonal and inter-annual variations in vegetation indices.
- **Band Values**: For each specified point within the study areas, spectral band values (such as near-infrared, red, and green) were extracted. These values are essential for calculating vegetation indices.
- **Scale Factors**: Applied to ensure consistency in the data, allowing for accurate comparison and analysis across different images and time periods.

### Spectral Indices

Spectral indices are mathematical combinations of different spectral band values, designed to highlight specific properties of vegetation:

- **NDVI Calculation**:
  - Normalized Difference Vegetation Index (NDVI) is a widely used index to measure vegetation health.
  - `NDVI = (NIR - Red) / (NIR + Red)`
  - *Explanation*: NDVI values range from -1 to 1, with higher values indicating healthier and denser vegetation.

- **EVI Calculation**:
  - Enhanced Vegetation Index (EVI) is an improvement over NDVI, reducing the impact of atmospheric effects.
  - `EVI = 2.5 * ((NIR - Red) / (NIR + 6 * Red - 7.5 * Blue + 1))`
  - *Explanation*: EVI is more sensitive to canopy structure and provides better contrast in areas of dense vegetation.

- **GNDVI Calculation**:
  - Green Normalized Difference Vegetation Index (GNDVI) focuses on the green band, offering a different perspective on vegetation health.
  - `GNDVI = (NIR - Green) / (NIR + Green)`
  - *Explanation*: GNDVI is particularly useful in assessing plant water content and chlorophyll concentration.

<img src="\preprint\Spectral indices.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />
<p style="text-align: justify;">


## Data Preprocessing


<img src="\preprint\pre pro.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />
<p style="text-align: justify;">

Before analysis, the data undergoes several preprocessing steps to ensure its quality and usability:

- **Spatial Smoothing**:
  - Apply a moving window spatial smoothing technique to reduce noise in NDVI values, which helps in better identifying trends.
  - Adjust the smoothing window size based on the density and variability of the vegetation in the study areas.
  - *Explanation*: This step is crucial for mitigating the effects of noise and outliers, which can distort the analysis results.

- **Outlier Removal**:
  - Identify and remove outliers using a z-score approach, where NDVI values are compared against the mean and standard deviation of the dataset.
  - Mask out any values that deviate significantly from the expected range (based on a chosen z-score threshold).
  - *Explanation*: Removing outliers ensures that the data used for analysis accurately represents typical vegetation conditions, improving the reliability of the results.

- **Time Series Analysis**:
  - Calculate the mean NDVI for each location over the selected time periods, providing a clear view of how vegetation changes over time.
  - Remove duplicate values to avoid skewing the analysis with redundant data points.
  - *Explanation*: Time series analysis allows for the observation of temporal patterns, such as seasonal growth cycles or the impact of environmental changes.

- **Data Cleaning (Python)**:
  - Use Python scripts to load, clean, and prepare the CSV datasets containing EVI and NDVI values.
  - Handle missing values and apply linear interpolation to fill in gaps, ensuring a complete dataset for analysis.
  - *Explanation*: Data cleaning is a critical step that ensures the integrity of the dataset, allowing for accurate and meaningful analysis.

<img src="\preprint\pre_pro_1 .png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />
<p style="text-align: justify;">


## Applying Wavelet Transform (WT)

Wavelet Transform (WT) is a powerful tool for analyzing time series data, particularly useful in detecting patterns across different frequency scales:

- **Wavelet Transform**:
  - Implement the WT using the `pywt` library in Python, which decomposes the time series into different frequency components.
  - This allows for the exploration of both high-frequency noise and low-frequency patterns, providing a more detailed understanding of the data.
  - *Explanation*: Wavelet Transform is especially effective in identifying cyclical patterns and trends that are not immediately apparent in the raw data.

- **Wavelet Coefficient Visualization**:
  - Plot the wavelet coefficients for each decomposition level, comparing mixed and pure teak vegetation types across different regions.
  - This visualization helps in understanding how different types of vegetation respond to environmental factors over time.
  - *Explanation*: By visualizing the coefficients, researchers can better identify significant patterns and anomalies in the vegetation data.

## Results and Insights

- **Denoising**:
  - After applying wavelet-based denoising techniques, the data reveals clearer trends and patterns, making it easier to identify key insights.
  - This process helps in isolating the true signal from noise, enhancing the accuracy of the analysis.
  - *Explanation*: Denoising is crucial for ensuring that the patterns observed in the data are reflective of actual vegetation dynamics, rather than being artifacts of noise.

<img src="\preprint\Result.png" alt="rapideye-intro" style="float: left; margin-right: 10px;" />
<p style="text-align: justify;">

*Explanation*: This image provides a visual summary of the key findings from the data analysis, highlighting how denoising has improved the clarity of the results.

## Conclusion

- **Methodology**:
  - The methodology used in this study, combining spectral indices, spatial smoothing, and wavelet transform, provides a comprehensive approach to analyzing Teak Tree Plantations.
  - This approach not only maps the plantations but also offers insights into their health and growth patterns.
  - *Explanation*: The methodology demonstrates the effectiveness of integrating multiple analytical techniques to achieve a deeper understanding of complex environmental data.

- **Impact**:
  - The findings of this study have significant implications for the management and conservation of Teak Tree Plantations, offering data-driven insights that can inform decision-making.
  - *Explanation*: By providing a clearer picture of plantation dynamics, this research can help stakeholders optimize management practices and ensure the sustainability of these valuable resources.

## Way Forward

- **Future Work**:
  - The next steps could include expanding the analysis to additional regions and exploring other vegetation types, using the same or enhanced methodologies.
  - Further research could also focus on integrating other remote sensing data sources, such as Sentinel-2, to complement the findings from Landsat 8.
  - *Explanation*: This section outlines potential directions for future research, emphasizing the scalability and adaptability of the methodologies developed in this study.


*Explanation*: This image illustrates potential areas for future research, encouraging further exploration and application of the methods developed in this project.

---