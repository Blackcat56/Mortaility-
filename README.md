# Mortaility-
##This code was used for a project for a python class
This code experiments with three ArcPro tools. 

Code example

python ```
import arcpy as ARCPY
aprx=arcpy.mp.ArcGISProject("CURRENT")
thisMap=aprx.listMaps()[0]
layerList=thisMap.listLayers()
MortalityLayer=thisMap.listLayers()[0]
#This group of code sees if the data is in the layer list or not (then it is added if it is not there). 
present=0
for layer in layerList:
    if (layer.name=="Stroke Mortality"):
        present= 1
    if present== 0:
        thisMap.addDataFromPath(ARCPY.GetParameter(0))
```
