# osm_completeness
This series of notebooks builds a model for predicting OSM building footprint area using random forest regression.

First, an [Observable notebook](https://observablehq.com/d/176fbd0640a04220) is used to identify those areas of a given region that already have complete OSM building mapping.Those areas are used as a training set for building the regression model. The order of notebook execution is:
1. `TrainOSM.ipynb` &mdash; create and save a random forest regression model that predicts OSM building footprint area
2. `SplitArea.ipynb` &mdash; create 250-m cell polygons over a region of interest from a shapefile
3. `Enrich.ipynb` &mdash; calculate features over each cell using Google Earth Engine and rasterio
4. `ApplyModel.ipynb` &mdash; run the regions of cells through the trained model to get predicted OSM footprint area

Results of `ApplyModel.ipynb` can be visualized in a second [Observable notebook](https://observablehq.com/d/09da0d4f932c9310). Be aware of a 15 MB limit to the file that can be attached to the notebook.

The conda environment necessary to run the Jupyter notebooks can be installed using the file `envs/wbenv.yml`. To build it enter:<br>
`conda env create --name envname --file wbenv.yml`<br>
in the the anaconda terminal when inside the directory containing the yml file. Windows machines may encounter an issue with the rtree dependency for geopandas, specifically the error `'OSError: could not find or load spatialindex_c-64.dll'` when importing geopandas. The workaround is to find the `core.py` file in the rtree library and change the line:<br>
```python
elif 'conda' in sys.version:
```
to
```python
elif os.path.exists(os.path.join(sys.prefix, 'conda-meta')):