# GISC420
### Initial Project Plan

#### Purpose: 
1) To use ArcGIS Pro's Model Builder to create a workflow that helps select a site based on the viewshed from a linear feature (such as a road)
2) To create a Python script that replicates this workflow through the use of Jupyter notebooks and code

Locations trialed have been selected based on the importance of viewsheds in the siting and location of development. 

#### Potential Locations to Trial: 
Wellington, New Zealand - we ended up choosing Wellington since there was a 1m DEM available and we both have experience with the area. 

#### Tasks
1) Find appropriate DEM
2) Create sample road lengths
3) Understand viewshed outputs
4) Think about workflow, combining raster
5) Trial in ArcGIS
6) Think about categorisation 
7) Experiment with different thresholds
8) Export code and look up pyarc 
9) Bring into Jupyter notebooks
10) Experiment and try and find pyarc solution

#### Potential Workflow
1) Inputs: DEM, linear feature, distance between points
2) Turn linear feature into points
3) Execute viewshed for each point
4) Combine resulting raster
5) Classify raster


#### Resources:
[arcpy code for a basic viewshed tool](https://pro.arcgis.com/en/pro-app/latest/tool-reference/spatial-analyst/viewshed.htm)

[ArcGIS Pro Course in viewshed analysis](https://www.esri.com/training/catalog/57d8718d8b3e1ff2376bf91c/performing-viewshed-analysis-in-arcgis-pro/)

[Iterations in Model Builder](https://pro.arcgis.com/en/pro-app/latest/tool-reference/modelbuilder-toolbox/examples-of-using-iterators-in-modelbuilder.htm) & [Youtube Walkthrough](https://www.youtube.com/watch?v=DoIkV2y0pEc)

[JupyterLab](https://www.youtube.com/watch?v=A5YyoCKxEOU&t=1s)


## Methods
Once we created a DEM for the Wellington City area and a .shp for a small roadlength to trial we started experimenting both within ArcGIS and using Python tools outside of ArcGIS. Some of our challenges and choices are discussed below:

### ArcGIS Pro Model Builder
We first used the Generate Points Along Lines tools and added Distance as an inputable parameter, along with the DEM file and the linear feature segement. We knew that we had to iterate through the points created in order to run viewshed for each of the points. We decided to use the Iterate Feature Selection tool which would go through and highlight each row(/point) within the dataset and run the viewshed. From there, we ran a visibility analysis on each point taking in the additional parameters of observer offset and surface offset to set how tall the observer and the future object would be from the DEM surface. This part of the iteration was successful and we were able to create a model that would create a viewshed raster for each point. 

We ran into issues at the "Append" stage. We tried using both the Append and Raster Calculator functions to add each raster to a final output raster. Since the iteration in ArcGIS doesn't allow as much control, and you are only able to use a single instance of iteration, we found that we were unable to append or add together datasets without resetting the original raster. This meant that the raster overwrote itself to its original empty values at the beginning of each loop. Although this was challenging, we were still able to create a tool that created a useful output, it just still requires a manual raster calculation as a last step.


### ArcGIS Pro Script in Spyder and Jupyter Notebooks/Lab

We exported the raw code and first brought it into Spyder and then into Jupyter notebooks to experiment with. In both instances the raw code would not run and we struggled to make arcpy work within the G420 environment, potentially due to licensing issues within the G420 environment. There were also lines of the code that were designed not to work once exported that we experimented with hashing out or removing. This all began a wider exploration into the challanges of using environments and packages within the confides of university computers which are partially locked down and do not allow for the download of most programs. 

### GDAL in Jupyter Lab

make sure to mention extracting the code to run GDAL Viewshed from QGIS Model Designer 

### GRASS in Jupyter Lab


### Cloning the ArcGIS environment, adding geopandas and working in Jupyter Lab






- First tried to create ArcGIS Pro ModelBuilder but ran into issues with only being able to do iteration once and not being able to choose how and when the iteration stopped in the workflow. We were able to use iteration for the points but were unable to combine the datasets within the workflow because the output raster kept on being overwritten
- We exported the code from Arc into Spyder to examine it, it had multiple errors and was unable to run as a standalone script
- We tried to bring that data into Jupyter Lab to work with and ran into the same issues
- Then we tried to get arcpy working on the lab machines but unfortunately ran into errors because of the environment and were unable to access the g420 environment and install arcpy on it
- We created a notebook with a breakdown of the process needed but still needed to resolve using a viewshed function
- We found a GDAL solution to running the viewshed analysis (insert hyperlink) but were unable to download the neccessary software on the lab computers
- We then tried to download and use a GRASS solution (insert hyperlink) but it just didn't work. 
