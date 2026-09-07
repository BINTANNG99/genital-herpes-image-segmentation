# Genital Herpes Image Segmentation

This project applies classical image-processing steps to a single genital-herpes image. The notebooks convert the image to grayscale, calculate an Otsu threshold, apply median filtering, remove small binary objects, label connected components, and extract region properties such as centroid, orientation, major-axis length, and minor-axis length.

`skimage.filters.threshold_otsu` computes a threshold from the image histogram using Otsu's method. The scikit-image documentation describes the method and its binary-thresholding use here: https://scikit-image.org/docs/stable/api/skimage.filters.html

## What the notebooks actually do

Most uploaded notebooks are variants of the same experiment. They are not independent projects.

The repeated input is:

```text
genital-herpes-simplex-6-transformed.jpeg
```

One variant instead reads `phase_separation.png`, and another uses a different herpes image path. The segmentation pipeline is broadly:

```text
image
-> grayscale
-> histogram
-> Otsu threshold
-> median filtering
-> binary threshold
-> remove small objects
-> fill small holes by inverse-mask filtering
-> connected-component labeling
-> region-property extraction
```

The more complete variants also export region measurements to Excel.

## Limitations

The notebooks process one image at a time and do not contain a labeled ground-truth mask. There is no Dice score, IoU, sensitivity, specificity, or other segmentation-quality metric. The output therefore demonstrates an image-processing procedure, not validated lesion segmentation accuracy.

Several thresholds are hard-coded:

- median-filter footprint: 7 × 7;
- `remove_small_objects(..., min_size=300)`;
- a later component-size rule based on bounding-box area below 2,000.

The notebooks do not contain an experiment showing that these values are optimal or transferable to other images.

One notebook attempts to inspect pixels with:

```python
phase_separation[390:410, 820:840]
```

even though the displayed image shape is 475 × 720 × 3. That column range is outside the image and the saved output reports `nan`. That inspection is invalid for that image.

A histogram is also calculated in some variants with `min=0, max=255` after `rgb2gray`, even though `rgb2gray` produces floating grayscale values. The earlier histogram using a 0-to-1 range is consistent with the actual representation; the 0-to-255 call is not.

The notebooks use private local or Google Drive paths in several cells. Those paths should be removed or parameterized before publication.

## Running

```bash
pip install -r requirements.txt
```

Provide an image whose use and redistribution are authorized, update the input path, and run the notebook cells in order.

## Reference

- scikit-image. `threshold_otsu` documentation. https://scikit-image.org/docs/stable/api/skimage.filters.html
