# Ges 486 Final Project

## Objectives

For this project, my aim was to represent health inequalities through a geospatial perspective. By using spatial statistics, as well as a series of stylistic maps, I wanted to crreate an analysis oflife expectancy and healthcare availability that was both analytical and approachable. In the United States, is there a relationship between the two?

## Methods and Data

To begin my analysis, I thought it would be appropriate to start on the state level. The first map I made was a series of simple cartograms with iterations varying from 1-7. In this map, the most bloated regions are those with the highest sq mile/ hospital ratio (furthest expected distance to travel to reciieve medical attention). This map also shows average life expectancy by state, with red being the lowest, and blue being the highest.

![animation2](https://user-images.githubusercontent.com/42807663/50254806-81080700-03bd-11e9-9b8d-f47b52f18508.gif)

Next, I chose  to create a chloropleth map of life expectancy in the United States, with blues and greens being the highest life expectancy, and oranges and reds being the lowest.

![ale1](https://user-images.githubusercontent.com/42807663/50254795-78afcc00-03bd-11e9-8363-c34df0c7175c.jpg)

Next, I chose to create a cloropleth of hospitals normalized to population. This would show what communities were being underserved as a relation of population density, rather than geographic distance.

![magmapph](https://user-images.githubusercontent.com/42807663/50255390-a85fd380-03bf-11e9-86fc-76fba8493e7b.jpg)

I was also aware that often times people travel beyond county and state boundries to recieve healthcare, so I created a simple hex map with 150 mile cells showing hospital distribution in the lower 48 states.

![hexmap](https://user-images.githubusercontent.com/42807663/50255386-a433b600-03bf-11e9-8073-67d4ebf6d768.jpg)

In addition to understanding general trends of health inequalities across the United States, I was also interested in knowing whether clusters of high or low life expectancy and hospital access existed to see if there were larger groups of underserved 

Here is a map for Univariate Spatial Autocorrelation performed on life expectancy data in the United States. In this map, dark red counties are those that are clustered with other high life expecancy counties, while those in blue represent spatial clusters of low life expectancy counties.
![ale_correlation](https://user-images.githubusercontent.com/42807663/50254845-9f6e0280-03bd-11e9-911f-c5b0e6d6068a.png)

Here is a map of significance for life expectancy in the US.
![ale_sig](https://user-images.githubusercontent.com/42807663/50257184-23c58300-03c8-11e9-8f5a-47820e0cb0d0.png)

This is a map showing the Spatial autocorrelation of hospital clusters across the United States
![correlation](https://user-images.githubusercontent.com/42807663/50254866-b280d280-03bd-11e9-810e-60f697edfd5a.png)

Here is the accompanying significance map.
![signif](https://user-images.githubusercontent.com/42807663/50257186-258f4680-03c8-11e9-84a5-991fff0c336a.png)

Finally, I wanted to know what counties in the US were underserved by county accoding to the national average of people/ hospitals. To do this, I used python to select the counties that were above and below this average.
![peopleperhospial](https://user-images.githubusercontent.com/42807663/50255402-b57cc280-03bf-11e9-8f90-a92d508bb989.jpg)

Here is my code:

'''python

#Adding counties with number of people per hospital
layer1 = QgsVectorLayer('A:/UMBC/Senior/GES486/FinalProject/Products/CountiesHOSPERPERS.shp','county','ogr')
layer1.isValid()
True
QgsProject.instance().addMapLayer(layer1)
<qgis._core.QgsVectorLayer object at 0x000001DB8068F5E8>
#Selecting counties with above average number of people served by each hospital
layer1 = iface.activeLayer()
layer1.selectByExpression("HosPerPers > 25446")
QgsVectorFileWriter.writeAsVectorFormat(layer1, r'A:/UMBC/Senior/GES486/FinalProject/Products/AboveAvPPH.gpkg', 'utf-8', layer1.crs(),'GPKG', True)
(0, '')
layer2 = QgsVectorLayer('A:/UMBC/Senior/GES486/FinalProject/Products/AboveAvPPH.gpkg','AbovAvPPH','ogr')
layer2.isValid()
True
QgsProject.instance().addMapLayer(layer2)
<qgis._core.QgsVectorLayer object at 0x000001DB80696F78>
#Repainting areas with poorer than average people per hospital to red
sym = QgsFillSymbol.createSimple({'color_expression':'Dif','color':'red'})
renderer = layer2.renderer()
renderer.setSymbol(sym)
layer2.triggerRepaint()
iface.layerTreeView().refreshLayerSymbology(layer2.id())
#Selecting counties with Below average number of people served by each hospital
layer1 = iface.activeLayer()
layer1.selectByExpression("HosPerPers < 25446")
QgsVectorFileWriter.writeAsVectorFormat(layer1, r'A:/UMBC/Senior/GES486/FinalProject/Products/BelowAvPPH.gpkg', 'utf-8', layer1.crs(),'GPKG', True)
(0, '')
layer3 = QgsVectorLayer('A:/UMBC/Senior/GES486/FinalProject/Products/BelowAvPPH.gpkg','AbovAvPPH','ogr')
layer3.isValid()
True
QgsProject.instance().addMapLayer(layer3)
<qgis._core.QgsVectorLayer object at 0x000001DB806A3B88>
#Repainting areas with better than average people per hospital to blue
sym = QgsFillSymbol.createSimple({'color_expression':'Dif','color':'blue'})
renderer = layer3.renderer()
renderer.setSymbol(sym)
layer3.triggerRepaint()
iface.layerTreeView().refreshLayerSymbology(layer3.id())

'''
