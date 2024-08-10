---
title: "Motor Vehicle Collisions in New York City Dashboard Project"
date: 2024-04-27T22:52:51+05:30
tags:  
    - Streamlit Dashboard
    - Data Visualization
    - GIS
    - Traffic Analysis
    - Interactive Visualization
    - Traffic Data Analysis
links:
url_code: 'https://github.com/Israelzemedkun/Streamlit-dashboard.git'
---

## 

### 1. **Introduction**

#### Background

<p style="text-align: justify;">
Motor vehicle collisions are a significant concern in urban areas, leading to injuries, fatalities, and property damage. Analyzing collision data can help in identifying patterns and preventing future incidents, ultimately improving road safety.

#### Objectives
<p style="text-align: justify;">
The objective of this project is to develop a Streamlit dashboard to visualize and analyze motor vehicle collisions in New York City. The dashboard will provide interactive tools for exploring collision data, identifying hotspots, and understanding temporal patterns.

#### Research Questions
- Where are the most injuries occurring in NYC?
- What times of day have the most collisions?
- Which streets are the most dangerous for different types of road users (pedestrians, cyclists, motorists)?

### 2. **Literature Review**

#### Review of Related Work
<p style="text-align: justify;">
Existing research on vehicle collision analysis often employs statistical methods and visualization tools to identify trends and patterns. Studies have used Geographic Information Systems (GIS) and other visualization tools to map collision hotspots and analyze temporal patterns. For example, static reports have identified dangerous intersections and peak collision times.

#### Identified Gaps
<p style="text-align: justify;">
Interactive dashboards like Streamlit offer real-time insights and facilitate better decision-making compared to static reports. They allow users to explore data dynamically, providing a more intuitive understanding of collision patterns.

### 3. **Methodology**

#### Data Description
<p style="text-align: justify;">
The dataset used is the "Motor Vehicle Collisions - Crashes" dataset from NYC's open data portal. It contains information on all reported collisions, including date, time, location, and the number of persons injured. Key preprocessing steps include:

```python
import pandas as pd

DATA_URL = "/path/to/Motor_Vehicle_Collisions_-_Crashes.csv"

def load_data(nrows):
    data = pd.read_csv(DATA_URL, nrows=nrows, parse_dates=[['CRASH_DATE', 'CRASH_TIME']])
    data.dropna(subset=['LATITUDE', 'LONGITUDE'], inplace=True)
    lowercase = lambda x: str(x).lower()
    data.rename(lowercase, axis='columns', inplace=True)
    data.rename(columns={'crash_date_crash_time': 'date/time'}, inplace=True)
    return data

data = load_data(100000)
original_data = data
```

- Reading the CSV file
- Parsing dates and times
- Handling missing values
- Renaming columns for consistency

#### Dashboard Features
<p style="text-align: justify;">
The dashboard includes the following features:

- Interactive maps to visualize injury locations
- Sliders to filter data by the number of injuries and time of day
- Bar charts to show breakdowns by minute and top dangerous streets

### 4. **Experimental Setup**

#### Environment
<p style="text-align: justify;">
The project was developed using Streamlit, pandas, NumPy, pydeck, and Plotly on a local machine. 

#### Parameters
- The data is loaded with a maximum of 100,000 rows for efficient performance.
- The map visualizations use specific parameters such as zoom level and hexagon layer settings to ensure clarity and interactivity.

```python
import streamlit as st
import numpy as np
import pydeck as pdk
import plotly.express as px

st.title("Motor Vehicle Collisions in New York City")
st.markdown("This application is a Streamlit dashboard that can be used to analyze motor vehicle collisions in NYC")

@st.cache(persist=True)
def load_data(nrows):
    data = pd.read_csv(DATA_URL, nrows=nrows, parse_dates=[['CRASH_DATE', 'CRASH_TIME']])
    data.dropna(subset=['LATITUDE', 'LONGITUDE'], inplace=True)
    lowercase = lambda x: str(x).lower()
    data.rename(lowercase, axis='columns', inplace=True)
    data.rename(columns={'crash_date_crash_time': 'date/time'}, inplace=True)
    return data

data = load_data(100000)
original_data = data
```

### 5. **Results**

#### Performance Metrics
<p style="text-align: justify;">
The dashboard performs efficiently, offering responsiveness and accurate visualizations. The interactivity allows for real-time data exploration, providing users with immediate insights into collision patterns.

#### Visuals
Key visualizations included in the dashboard:

- **Map showing injury locations**: Users can filter the map by the number of injuries.

```python
st.header("Where are the most people injured in NYC?")
injured_people = st.slider("Number of persons injured in vehicle collisions", 0, 19)
st.map(data.query("injured_persons >= @injured_people")[["latitude", "longitude"]].dropna(how="any"))
```
<img src="/No person injury.png"
     alt="AOD average"
     style="float: left; margin-right: 10px;" />

- **Time-based collision analysis**: A slider allows users to explore collision data by hour of the day.

```python
st.header("How many collisions occur at a given time of day?")
hour = st.slider("Hour to look at", 0, 23)
data = data[data['date/time'].dt.hour == hour]

st.markdown("Vehicle collisions between %i:00 and %i:00" % (hour, (hour + 1) % 24))
midpoint = (np.average(data['latitude']), np.average(data['longitude']))

st.write(pdk.Deck(
    map_style="mapbox://styles/mapbox/light-v9",
    initial_view_state={
        "latitude": midpoint[0],
        "longitude": midpoint[1],
        "zoom": 11,
        "pitch": 50,
    },
    layers=[
        pdk.Layer(
            "HexagonLayer",
            data=data[['date/time', 'latitude', 'longitude']],
            get_position=['longitude', 'latitude'],
            radius=100,
            extruded=True,
            pickable=True,
            elevation_scale=4,
            elevation_range=[0, 1000]
        ),
    ]
))
```
<img src="/No-collision_24.png"
     alt="AOD average"
     style="float: left; margin-right: 10px;" />


- **Breakdown by minute**: Bar charts show the number of collisions by minute within a selected hour.

```python
st.subheader("Breakdown by minute between %i:00 and %i:00" % (hour, (hour + 1) % 24))
filtered = data[(data['date/time'].dt.hour >= hour) & (data['date/time'].dt.hour < (hour + 1))]
hist = np.histogram(filtered['date/time'].dt.minute, bins=60, range=(0, 60))[0]
chart_data = pd.DataFrame({'minute': range(60), 'crashes': hist})
fig = px.bar(chart_data, x='minute', y='crashes', hover_data=['minute', 'crashes'], height=400)
st.write(fig)
```
<img src="/Break_down_by_minute.png"
     alt="AOD average"
     style="float: left; margin-right: 10px;" />

- **Top dangerous streets**: Lists of the top 5 dangerous streets for pedestrians, cyclists, and motorists.

```python
st.header("Top 5 dangerous streets by affected type")
select = st.selectbox('Affected type of people', ['Pedestrians', 'Cyclists', 'Motorists'])

if select == 'Pedestrians':
    st.write(original_data.query("injured_pedestrians >= 1")[["on_street_name", "injured_pedestrians"]].sort_values(by=['injured_pedestrians'], ascending=False).dropna(how='any')[:5])

elif select == 'Cyclists':
    st.write(original_data.query("injured_cyclists >= 1")[["on_street_name", "injured_cyclists"]].sort_values(by=['injured_cyclists'], ascending=False).dropna(how='any')[:5])

elif select == 'Motorists':
    st.write(original_data.query("injured_motorists >= 1")[["on_street_name", "injured_motorists"]].sort_values(by=['injured_motorists'], ascending=False).dropna(how='any')[:5])
```

<img src="/Top dangerous streets.png"
     alt="AOD average"
     style="float: left; margin-right: 10px;" />


### 6. **Discussion**

#### Analysis
<p style="text-align: justify;">
The dashboard provides valuable insights into collision hotspots, peak times for accidents, and the most dangerous streets for different road users. For instance, certain areas in Manhattan and Brooklyn are identified as high-risk zones.

#### Limitations
<p style="text-align: justify;">
- The dataset may contain incomplete or inaccurate data, which could affect the analysis.
- Performance limitations arise when loading large datasets into a web application, necessitating the use of a data subset.

#### Comparisons
<p style="text-align: justify;">
Insights from the dashboard can be compared with existing traffic safety reports or studies to validate findings and highlight areas needing further investigation.

### 7. **Conclusion**

#### Summary
<p style="text-align: justify;">
Key findings include the identification of high-risk areas and times for collisions, demonstrating the effectiveness of interactive visualizations in understanding traffic safety data.

#### Future Work
- Integrating real-time data to keep the dashboard up-to-date
- Expanding the analysis to other cities
- Enhancing the dashboard with predictive analytics to forecast future collision hotspots

### 8. **References**
- **Data Source**: NYC Open Data - Motor Vehicle Collisions - Crashes
- **Documentation**: Streamlit, pandas, NumPy, pydeck, and Plotly libraries

### 9. **Appendices**

#### Additional Data
<p style="text-align: justify;">
Include any supplementary material relevant to your research, such as raw data files or additional charts and tables that support the analysis.
