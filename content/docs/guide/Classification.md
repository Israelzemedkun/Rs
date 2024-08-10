---
title: Accuracy Assessment
weight: 3
---

## Classification Validation

### Objective
The objective of this lab is to learn how to evaluate image classification results and conduct an accuracy assessment using independent validation data.

### Recap - Loading an Image
For a given region of interest (`roi` - defined by a point geometry), filter the Landsat-8 image collection for a date range and extract a cloud-free image (`image`).

```javascript
var image = ee.Image(ee.ImageCollection('LANDSAT/LC08/C01/T1_SR')
    .filterBounds(roi)
    .filterDate('2016-05-01', '2016-06-30')
    .sort('CLOUD_COVER')
    .first());
```

Load a true-color composite to the map view:

```javascript
Map.addLayer(image, {bands: ['B4', 'B3', 'B2'], min: 0, max: 3000}, 'True colour image');
```

### Recap - Supervised Classification
Use the rectangle geometry tool to collect training data for four different landcover types (e.g., `tWater`, `tCity`, `tForest`, `tOther`). Remember to change the Import type to `FeatureCollection`, and add a common property such as `'landcover'` with an integer label starting at 0. Ensure that your total sampled area is less than 5000 pixels.

Merge your training data:

```javascript
// Merge into one FeatureCollection and print details to console
var classNames = tWater.merge(tCity).merge(tForest).merge(tOther);
print(classNames);
```

Sample the bands within your training data polygons:

```javascript
// Extract training data from select bands of the image, print to console
var bands = ['B2', 'B3', 'B4', 'B5', 'B6', 'B7'];
var training = image.select(bands).sampleRegions({
  collection: classNames,
  properties: ['landcover'],
  scale: 30
});
print(training);
```

Train the classifier:

```javascript
// Train classifier - e.g., cart, randomForest, svm
var classifier = ee.Classifier.cart().train({
  features: training,
  classProperty: 'landcover',
  inputProperties: bands
});
```

Classify the input image:

```javascript
// Run the classification
var classified = image.select(bands).classify(classifier);
```

Add the classified map to the map view:

```javascript
// Centre the map on your training data coverage
Map.centerObject(classNames, 11);
// Add the classification to the map view, specify colours for classes
Map.addLayer(classified,
{min: 0, max: 3, palette: ['blue', 'red', 'green', 'yellow']},
'classification');
```

### Classification Validation
Collect validation data using the rectangle polygon geometry tool:

- Do this in the same way you collected training data.
- Use the same property names and labels.
- Do not overlap the training data.
- Do not exceed 5000 pixels.
- Collect examples of the same four classes but name them differently (`vWater`, `vCity`, `vForest`, `vOther`).

Merge your validation polygons into one FeatureCollection:

```javascript
// Merge into one FeatureCollection
var valNames = vWater.merge(vCity).merge(vForest).merge(vOther);
```

Sample your classification results to your new validation areas:

```javascript
var validation = classified.sampleRegions({
  collection: valNames,
  properties: ['landcover'],
  scale: 30,
});
print(validation);
```

Run the validation assessment using the error matrix approach:

```javascript
// Compare the landcover of your validation data against the classification result
var testAccuracy = validation.errorMatrix('landcover', 'classification');
// Print the error matrix to the console
print('Validation error matrix: ', testAccuracy);
// Print the overall accuracy to the console
print('Validation overall accuracy: ', testAccuracy.accuracy());
```

### Export Your Error Matrix
For further analysis, export your error matrix to calculate individual class accuracies and User's and Producer's accuracy.
