
This guide provides a step-by-step walkthrough for converting GeoJSON files into 3D Tiles that are compatible with CesiumJS. All tools used are free and open-source.
The process outlined here is based on my own experience, including common issues and their solutions.

#Tools Required
- Python	3.8 to 3.12 only	Required to run the Py3dtilers conversion script
- Py3dtilers	The main utility for GeoJSON → 3D Tiles
- QGIS	Any recent version	Used to inspect, clean, and reproject your GeoJSON data

#Key Concepts

📄 What is GeoJSON?
GeoJSON is a widely used format for encoding geographic data structures using JavaScript Object Notation (JSON). It is lightweight and human-readable, often used for sharing map data.

🧱 What are 3D Tiles?
3D Tiles is an open specification developed by Cesium to stream and render large-scale 3D geospatial datasets such as buildings, point clouds, or photogrammetry models efficiently.


###Step 1: Prepare Your GeoJSON Data
Download your data (e.g., from OneGeo or another source).
Open the file in QGIS.
Reproject the data to EPSG:4087 (WGS 84 / NSIDC Sea Ice Polar Stereographic North):
Go to: Layer > Export > Save Features As…
Set CRS to EPSG:4087
Save the reprojected file.

Why EPSG:4087?
This projection ensures that Py3dtilers interprets your coordinates correctly for conversion to global 3D Tiles.

NOTE: The conversion is important as Cesium is a 3d software that uses different projections than 2d used by OneGeo
Open PowerShell or your preferred terminal.


###Step 2.: Run the following command on (replace paths accordingly):
geojson-tiler -i "C:\Path\To\Your\file.geojson" -o "C:\Path\To\OutputFolder" --height height --crs_in EPSG:4087 --crs_out EPSG:4978
- `--height` is the attribute in your GeoJSON that contains height/elevation information (e.g., "height" or "base_height").
- `--crs_out EPSG:4978` converts to Earth-Centered, Earth-Fixed (ECEF), which Cesium uses internally.

Common Error & Fix:
You may encounter the following error during conversion:
TypeError: GltfPrimitive.__init__() got an unexpected keyword argument 'normals'
To fix this:
- Refer to the GitHub issue and solution: https://github.com/Oslandia/py3dtilers/issues/199


###Step 3: Upload to Cesium
Your output folder will contain:
- `.b3dm` (Binary 3D Model) files
- `tileset.json`
Zip the folder containing these files.
Upload the ZIP file to Cesium Ion for visualisation




