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


## Methods & Discussion
We initially downloaded the Wellington LiDAR 1m DEM from Land Information New Zealand. We created a mosaic DEM from the rasters provided. When experimenting with Viewshed using the DEM we found the analysis took a long time to run due to the extent of the DEM. For this reason, we chose to clip the DEM and perform our analysis in a smaller area. We chose an area close to the centre of Wellington which we are both familiar with. This should help us when interpreting the accuracy of the viewshed analysis we generate.    

Once we created a [DEM for the Wellington City area and a .shp for a small roadlength](https://github.com/rhiannonbar/GISC420/blob/fcf0f14e5222b6bf6185d1670432550e50b411bc/Final%20Project%20Initial%20Data.zip) to trial we started experimenting both within ArcGIS and using Python tools outside of ArcGIS. Some of our challenges and choices are discussed below:

### ArcGIS Pro Model Builder
We first used the Generate Points Along Lines tools and added Distance as an inputable parameter, along with the DEM file and the linear feature segment. Using distance as an model parameter means the number of points (and the number of viewshed's generated) will change based on the length of the linear feature that is fed into the model. We set a minimum of 5m for the distance as viewshed analysis can be computationally challenging and take a long time. We chose not to apply a limit on maximum distance between points to try and make the tool more versatile. For example, we would want to have a much greater distance when placing points along a highway compared with a smaller road in a housing development. We knew that we had to iterate through the points created in order to run viewshed for each of the points. We decided to use the Iterate Feature Selection tool which would go through and highlight each row(/point) within the dataset and run the viewshed. From there, we ran a visibility analysis on each point taking in the additional parameters of observer offset and surface offset to set how tall the observer and the future object would be from the DEM surface. This part of the iteration was successful and we were able to create [a model that would create a viewshed raster for each point.](https://github.com/rhiannonbar/GISC420/blob/fcf0f14e5222b6bf6185d1670432550e50b411bc/ArcGISModelGraphic.svg)

We ran into issues at the "Append" stage. We tried using both the Append and Raster Calculator functions to add each raster to a final output raster. Since the iteration in ArcGIS doesn't allow as much control, and you are only able to use a single instance of iteration, we found that we were unable to append or add together datasets without resetting the original raster. This meant that the raster overwrote itself to its original empty values at the beginning of each loop. Although this was challenging, we were still able to create a tool that created [a useful output](https://github.com/rhiannonbar/GISC420/blob/main/OutputExample.pdf), it just still requires a manual raster calculation as a last step. We also experimented by grouping the components within the model to see if we could perform multiple individual steps within one model but didn't have any success. This experience highlighted the limitations of attempting to perform more complex tasks using model builder.

### ArcGIS Pro Script in Spyder and Jupyter Notebooks/Lab

We exported the [raw code](https://github.com/rhiannonbar/GISC420/blob/fcf0f14e5222b6bf6185d1670432550e50b411bc/ArcGIS_Raw_Python.ipynb) and first brought it into Spyder and then into Jupyter notebooks to experiment with. In both instances the raw code would not run and we struggled to make arcpy work within the G420 environment, potentially due to licensing issues within the G420 environment. There were also lines of the code that were designed not to work once exported that we experimented with hashing out or removing. This all began a wider exploration into the challenges of using environments and packages within the confides of university computers which are partially locked down and do not allow for the download of most programs. 

### GDAL in Jupyter Lab

Having struggled to use arcpy within the G420 environment, we researched other library available in python that would let us perform viewshed analysis on a test point. We came across [github documentation](https://github.com/jonnyhuck/Viewshed) for viewshed analysis using the gdal package. Whilst the package itself was compatible with the G420 environment, we struggled to access [OsGeo4W](https://trac.osgeo.org/osgeo4w) needed to run the viewshed analysis via the lab computers. 

We also checked QGIS and used its Model Designer to examine the code used for its GDAL-based viewshed tool. Unfortunately, we were again unable to download the appropriate packages.

### GRASS in Jupyter Lab

Looking for another solution, we tried to use the r.viewshed function from [Grass GIS](https://grass.osgeo.org/grass78/manuals/r.viewshed.html). Again we struggled to access this through the G420 environment. 

### Cloning the ArcGIS Environment, Adding Geopandas and Working in Jupyter Lab

Through experimentation, we realised that the Python tab of ArcPro settings allows the user to clone their ArcGIS environment and add additional modules such as geopandas and rasterio. The original ArcGIS environment is locked down and limited in its customisation, however this new environment was in a location that allowed access via Anaconda and Jupyter Notebook. This approach allowed us to use arcpy packages alongside other more python friendly packages such as geopandas. 

As a result, we were able to put together a workflow using a Jupyter Notebook. We reviewed the raw code again from Model Builder and reused some of the arcpy functions with minor alterations. First we used the linear feature (roadseg) we had generated in Arcmap to test the GeneratePointsAlongALine function in arcpy. The code ran well and generated points along the linear feature. We then tried to run a viewshed on a test point, again using the Arcpy code. We input our DEM and test point into the function and [generated an output](https://github.com/rhiannonbar/GISC420/blob/main/GEOG420%20Final%20Project.ipynb). 

Once of the major challenges with using arcpy is that the functions we used were extremely complex and we spent a considerable amount of time working through syntax errors. The outputs also didn't save to the environment so we were creating a significant number of files and ran into issues saving files in a consistent location. This issue is resolvable but requires more time than we had available to make file saving consistent and replicatable . 

Finally, we attempted to create a loop for generating points and performing viewshed analysis for all of the points along a line. After many attempts to work within arcpy we realised that iteration using only arcpy was not feasible for us at this time. We decided that a combination of geopandas and arcpy would be the most achievable method. This method is described below:
  1) Convert the points layer to a geopandas geodataframe
  2) Iterate through each feature in the geodataframe
  3) Convert the feature into a .shp file and save
  4) Perform the viewshed analysis
  5) Append the results
  6) Repeat iteration until points layer is complete
 
 Unfortunately, we were not able to complete the loop portion of this code within the time constraints of this class. Despite the challenges of changing data formats and arcpy "wrangling", we believe that this method could be more appropriate to create a function that performs this analysis.


## Limitations of Methodology 
- We only tested our data using a LiDAR based DSM and we also only used one line segment. We could perform the same method on different regions, such as areas with less elevation change, and see how this effected our output. We also didn't the same method on longer line segments. As viewshed analysis is computationally challenging, there will be a limit on how many viewsheds should be performed per model in model builder. We could have used different line segments to explore this further    
-  We performed our viewshed analysis using a DEM rather than a DSM. A DSM includes above ground features such as buildings and vegetation. In an urban environment such as Wellington, above ground features are likely to exert a strong control on what is visible in a landscape. However, a DSM for the Wellington region would be more challenging to obtain. Viewshed analysis also doesn’t consider how visibility in the landscape may change over time due to factors such as seasonal changes in vegetation. 


## Challenges 
- The main challenge we found was developing a method that would allow us to automate the viewshed of each individual point.  
- Another challenge we ran into was developing a method that would allow us to automate the viewshed of each individual point. In addition, we struggled to create environments to give us access to the libraries we needed. This was likely due to restrictions in accessing arcpy outside of the ESRI environment and restrictions in altering environments on the lab computers. 

## Future development 
-	The availability of viewshed functions in GDAL would enable use to reproduce our methodology without relying on acrpy code. Using open source methods outside of the ESRI environment may enable us to have more flexibility in utilising different environments. We would need to try this on computers outside of the lab with less restrictions on creating and altering environments. 
- In future we could also think about how to improve the classification of our output. We could reclassify the output into high, medium and low visibility as this may be more intuitive to understand than "visible at x no. of points" 
- As previously discussed, viewshed analysis is computationally challenging, especially in Arc and can take a long time. It may be interesting to explore if the same issues are present when working in python. Is there a way to store each viewshed as an object within the function rather than writing it out to a file and then adding them together?
