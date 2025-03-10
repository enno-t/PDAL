(workshop-dtm)=

# Generating a DTM

```{index} elevation model, DTM, DSM
```

This exercise uses PDAL to generate an elevation model surface using the
output from the {ref}`workshop-ground` exercise, PDAL's {ref}`writers.gdal`
operation, and {{ GDAL }} to generate an elevation and hillshade surface from
point cloud data.

## Exercise

```{note}
The primary input for [Digital Terrain Model] generation is a point cloud
with ground classifications. We created this file, called
`denoised-ground-only.laz`, in the {ref}`workshop-ground` exercise. Please produce that
file by following that exercise before starting this one.
```

### Command

PDAL capability to generate rasterized output is provided by the {ref}`writers.gdal` stage.
There is no {ref}`application <apps>` to drive this stage, and we must use a pipeline.

### Pipeline breakdown

```{eval-rst}
.. include:: ./gdal.json
    :literal:
```

```{note}
This pipeline is available in your workshop materials in the
`./exercises/analysis/dtm/gdal.json` file. Make sure to edit the
filenames to match your paths.
```

#### 1. Reader

`denoised-ground-only` is the [COPC](https://copc.io) file we will clip. You should have
created this output as part of the {ref}`workshop-ground` exercise.

#### 2. {ref}`writers.gdal`

The {ref}`writers.gdal` writer that bins the point cloud data into an elevation
surface.

### Execution

```console
$ pdal pipeline ./exercises/analysis/dtm/gdal.json
```

### Visualization

Something happened, and some files were written, but we cannot really
see what was produced. Let us use {ref}`qgis` to visualize the output.

1. Open {ref}`qgis` and `Add Raster Layer`

2. Add the dtm.tif file from your `./exercises/analysis/dtm`
   directory

   > ```{image} ../../images/dtm-add-raster-mean.png
   > :target: ../../../_images/dtm-add-raster-mean.png
   > ```
   >
   > ```{image} ../../images/dtm-qgis-added-no-fill.png
   > :target: ../../../_images/dtm-qgis-added-no-fill.png
   > ```

3. Go to Raster -> Analyze -> Fill nodata... and select the default values

   > ```{image} ../../images/dtm-qgis-fill-nodata.png
   > :target: ../../../_images/dtm-qgis-fill-nodata.png
   > ```
   >
   > ```{image} ../../images/dtm-qgis-added.png
   > :target: ../../../_images/dtm-qgis-added.png
   > ```

4. Classify the DTM by right-clicking on the `Filled` and choosing
   `Properties`. Pick the singleband pseudocolor for the rendering type, and then
   choose a color ramp and click `Classify`

   > ```{image} ../../images/dtm-qgis-colorize-dtm.png
   > :target: ../../../_images/dtm-qgis-colorize-dtm.png
   > ```

5. {ref}`qgis` provides access to {{ GDAL }} processing tools, and we
   are going to use that to create a hillshade of our surface.
   Choose `Raster-->Analysis-->Hillshade`

   > ```{image} ../../images/dtm-qgis-select-hillshade.png
   > :target: ../../../_images/dtm-qgis-select-hillshade.png
   > ```

6. Click the window for the `Output file` and select a location
   to save the `hillshade.tif` file

   > ```{image} ../../images/dtm-qgis-gdaldem.png
   > :target: ../../../_images/dtm-qgis-gdaldem.png
   > ```

7. Click `OK` and the hillshade of your DTM is now available

   > ```{image} ../../images/dtm-qgis-hillshade-done.png
   > :target: ../../../_images/dtm-qgis-hillshade-done.png
   > ```

## Notes

1. [gdaldem], which powers the {ref}`qgis` DEM tools, is a very powerful
   command line utility you can use for processing data.
2. {ref}`writers.gdal` can be used for large data, but it does not interpolate
   a typical [TIN] surface model.
3. If you are missing the Raster tab, check your plugins in QGIS. If you have none
   : you may need to reinstall QGIS.

[digital terrain model]: https://en.wikipedia.org/wiki/Digital_elevation_model
[gdaldem]: http://www.gdal.org/gdaldem.html
[tin]: https://en.wikipedia.org/wiki/Triangulated_irregular_network
