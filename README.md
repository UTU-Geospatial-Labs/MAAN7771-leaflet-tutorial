# Leaflet webmap tutorial for MAAN7771 | 2025

Leaflet JavaScript library is an accessible resource for creating interactive webmaps. This tutorial creates a simple Leaflet webmap that draws layers from different data sources: Maptiler, OSM and our course Geoserver. The demo webmap is accessible live here: [https://voaaal.github.io/Leaflet-WebMap/](https://voaaal.github.io/Leaflet-WebMap/).

This webmap has been created with the support of Leaflet Tutorials:
- [Leaflet Quick Start Guide](https://leafletjs.com/examples/quick-start/),
- [Leaflet WMS and TMS](https://leafletjs.com/examples/wms/wms.html),
- [Layer Groups and Layers Control](https://leafletjs.com/examples/layers-control/)

## 1. Initiating Leaflet application
All you need is a code editing software, such as Notepad++, Visual Studio Code or similar that allows you to edit HTML files.

Create an empty `index.html` file. In case you use a code editor with more advanced features, you can add a basic HTML file structure to the empty file. For example, in Visual Studio Code this happens by typing `!` at the beginning of the empty file and hitting `Enter`. VScode also provides the possibility to see your changes in the source code live by running it in browser after changes (Run --> Start debugging OR Run --> Run without debugging OR CTRL+SHIFT+D).

Start populating your file with Leaflet code. It's critical that you provide links to Leaflet libraries at the beginning of your file, inside the `<head>...</head>`section (see [Leaflet Quick Start Guide](https://leafletjs.com/examples/quick-start/)).

## 2. Data sources in the demo webmap

### OpenStreetMap
OpenStreetMap basemap can be added as a `tileLayer` with the following code:
```javascript
var OpenStreetMap = L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
            attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
        }).addTo(map);
```
Remember to always add attribution to the map when using OSM basemaps or data!

### Maptiler
Maptiler provides myriad of basemaps to be used in webmaps. Maptiler requires signing in, which grants you an unique key that must be included in the link when drawing their resources to webmaps. They also provide ready-to-use code snippets for Lealflet (and OpenLayers) webmaps.
```javascript
var satelliteimagery = L.tileLayer('https://api.maptiler.com/maps/satellite/{z}/{x}/{y}.jpg?key=YOUR-UNIQUE-KEY', {
            attribution: '<a href="https://www.maptiler.com/copyright/" target="_blank">&copy; MapTiler</a>'
        }).addTo(map);
```
### Geoserver
WMS layers can be called from various sources providing them, such as Geoservers. In this examples, the layer "Tanzania forest reserves 2020" from Workspace "Tanzania" is called. Few parameters has been defined. The parameter `transparent` is set `true` to prevent no-data areas displaying as white. Instead, they are now transparent. `format` of WMS data is image.
```javascript
var forestreserves = new L.tileLayer.wms('https://geo-maa.utu.fi/geoserver/wms', {
            layers: 'tanzania_forest_reserves_2020',
            transparent: true,
            format: 'image/png'
        }).addTo(map);
```
## 3. Hosting on GitHub Pages
The `index.html` file can be added to the `main` branch root in GitHub as it is. In case you use some local data files (e.g. GeoJSON or Shapefile) or additional resources, those must be also brought to GitHub in their corresponding folders. A GitHub Page can then be initiated from the `/root` folder of your  `main` branch.

## Full code
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tanzania forest reserves map</title>

    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
    integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY="
    crossorigin=""/>

    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"
    integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo="
    crossorigin=""></script>

    <style>
        #map { 
            position: absolute; 
            top: 0; 
            bottom: 0; 
            left: 0; 
            right: 0; }
    </style>
</head>
<body>
    <div id="map"></div> 
    <script>
        var map = L.map('map').setView([-6.13, 35.09], 7);

        //Layers
        var satelliteimagery = L.tileLayer('https://api.maptiler.com/maps/satellite/{z}/{x}/{y}.jpg?key=F651PQ3V3To4VcaQBjAv', {
            attribution: '<a href="https://www.maptiler.com/copyright/" target="_blank">&copy; MapTiler</a>'
        }).addTo(map);

        var OpenStreetMap = L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
            attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
        }).addTo(map);

        var forestreserves = new L.tileLayer.wms('https://geo-maa.utu.fi/geoserver/wms', {
            layers: 'tanzania_forest_reserves_2020',
            transparent: true,
            format: 'image/png'
        }).addTo(map);


        //Layer control
        var baseMaps = {
            "OSM": OpenStreetMap,
            "Imagery": satelliteimagery
        };

        var overlayMaps = {
            "Forest reserves": forestreserves
        };

        L.control.layers(baseMaps, overlayMaps).addTo(map);

    </script>
</body>
</html>
```
