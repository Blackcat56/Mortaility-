# Stroke Mortaility
## This code was used for a project for a python class
This code experiments with three ArcPro functions. 

## Code example

```python


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
## Function:
1. Bar chart

```python
#This is the first ArcPy Function:
#Chart code (in a group) It works and the graph can be seen in the chart area in the contents. 
Mortalitychart=thisMap.listLayers("Stroke Mortality")[0]
#Have to make sure the .Chart is capitalized. It will not work otherwise.  
ch=arcpy.Chart("Chart 1")
ch.type='bar'
ch.title='Female Stroke Mortalities'
ch.xAxis.field='county'
ch.xAxis.title='Countries'
ch.yAxis.field='ALL_F_ALL'
ch.yAxis.title='All Female stroke mortality'
ch.addToLayer(Mortalitychart)
```

2. Selecting by attribute

```python
#This is the second ArcPy Function.
#Selecting by attribute. For this code I looked at Everyone's Mortality above 50. 
Selected=arcpy.SelectLayerByAttribute_management('Stroke Mortality', 'NEW_SELECTION','"ALL_F_ALL">50.0', 'NON_INVERT')
#This code create a new layer from the selected created from above
AllSel=arcpy.CopyFeatures_management(Selected,"All_Selected_50")
#This puts the above varible into another varible. 
lyr=AllSel
#This adds the varible to the current map.  
thisMap.addDataFromPath(lyr)

#The second Symbology once the other layer is added. 
All=thisMap.listLayers()[0]
All.symbology=Allsy
Allsy=All.symbology
Allsy.updateRenderer('GraduatedColorsRenderer')
All.symbology=Allsy
Allsy.renderer.classificationField="ALL_F_ALL"
All.symbology=Allsy
Allsy.renderer.colorRamp=aprx.listColorRamps("Cyan to Purple")[0]
All.symbology=Allsy
```
3. Table 

```python
#This is the third Arcpy Function.  
#This code looks at the mean of the All field using the statistical analysis.
#the two varables for the Statistics_analysis. 
out_table="in_memory\stats_table"
StatField=['ALL_F_ALL','MEAN'],['ALL_F_ALL','MAX']
#This is the create the table (you can find this at list at selection on the contents). 
table=arcpy.Statistics_analysis('Stroke Mortality',out_table,StatField)
```
