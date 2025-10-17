# Create a case
In this section, we will introduce how to create a road network from OSM Web Wizard.

## Introduction
OSM Web Wizard is a tool provided by SUMO to capture road network from OpenStreetMap. It is not specially designed for V2Sim. 

**Note**: Due to network restrictions, **users in China Mainland** may not able to visit OpenStreetMap.

## Open OSM Web Wizard
OSM Web Wizard usually locates in the folder of SUMO.

![osm_place](https://github.com/user-attachments/assets/b7854f3a-f087-4ea1-b7c5-38b4d643e738)

You can also directly search it in the "Start" menu to open it.

## Locate your desired area

You can input the city name or use latitude and longitude to locate your desired area. By default, it will download the whole area you see. If you just want part of the area, tick `Select Area` and drag the box border to select your preferred area.

![osm1](https://github.com/user-attachments/assets/bdb1fdf5-529c-4813-b7b6-1e1531828425)

In the "Options" column, tick "Add Polygons" since they are used in EVCS and trip generation.

## Clear the trips
Deselect all the checkbox to prevent any trip from generation here. Trips generation should be done in V2Sim since it follows different protocol compared to SUMO.

![osm2](https://github.com/user-attachments/assets/9af8d30e-620d-4b87-a1f8-382a911d4175)

## Select proper edges
Only "highway" should be preserved. Preserve other way is unnecessary and may decrease the performance.

![osm3](https://github.com/user-attachments/assets/cefa9d20-893f-4b8e-b9c8-68400860d618)

## Generate your case
Click the green button "Generate Scenario" to create your case. After that, copy your case generated with OSM Web Wizard to a proper place, such as the `case` folder in your downloaded source coded. Then run `gui_main.py` in the command prompt by:
```bash
python gui_main.py
```
* **Notice:** Double click to open this file is **NOT** recommended since many users have reported it doesn't work as expected.

Click the `Project` menu and open the folder you have created. 
