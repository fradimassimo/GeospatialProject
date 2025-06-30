# 🏙️ WCI – Walkability Composite Index  
### *Francesco Di Massimo*

Urban walkability can be understood and measured in many different ways, which makes the concept inherently multifaceted. A walkable environment might be defined by a dense and connected street network, or by the availability of diverse services within a short walking distance.

However, other factors — such as the **morphological characteristics** of the urban landscape — can also significantly influence both **walkability**, especially in topographically complex cities like Rome.

This project aims to evaluate the **walkability of the city of Rome** by analyzing a set of spatial metrics derived from **OpenStreetMap (OSM)** data and **satellite elevation data** from the **Copernicus Digital Elevation Model (COP-DEM)**. These metrics are then combined into a **composite accessibility index (WCI)** to identify which municipalities of Rome are more — or less — accessible by foot. 
> ![overview Map](output/graph_overview.png)

---
## 📁 Repository Structure
- **Walkability-Rome/**
  - **data/**  
    - **slope_d/**  
    - **connectivity_d/**  
    - **15min_d/**  
  - **notebooks/**  
    - `connectivity.ipynb`  
    - `15min_score.ipynb`  
    - `slope.ipynb`  
  - **outputs/**  
  - `README.md`  
  - `requirements.txt`  
---

## Installation Notes
This project was developed with **Python 3.11** and includes both vector and raster spatial analysis tools. To ensure full reproducibility and compatibility, follow the instructions below.\


### ⚙️ GDAL Installation
GDAL has to be installed on your system.
GDAL is required for raster processing and command-line tools like `gdalwarp` and `gdaldem` used in this project.

### On Ubuntu/Debian:
sudo apt-get update\
sudo apt-get install gdal-bin libgdal-dev


### On macOS(Homebrew):
brew install gdal

### On Windows(conda):
conda install -c conda-forge gdal

### Python Environment Setup
Once GDAL is installed:

python -m venv venv\
source venv/bin/activate       # or venv\Scripts\activate on Windows\
pip install -r requirements.txt


---

## 📌 Project Structure
The project is organized into separate notebooks, each addressing a different component of walkability through a specific methodological approach. Every notebook focuses on a distinct spatial dimension — connectivity, accessibility, and morphology — and concludes with a visualization of Rome’s municipalities based on that particular criterion.

The final notebook, focused on slope and urban morphology, also includes the construction of the composite Walkability Index, which combines the previous metrics into a single indicator.

### 1. Connectivity (`connectivity.ipynb`)
- Download and preprocess **OSM street network** using osmnx  
- Project and clip the network to the administrative boundaries of Rome’s **municipalities**  
- Perform cleaning of neighborhoods to ensure smoother analysis  
- Initially replicate the first part of the walkability analysis from [this project](https://github.com/eemilhaa/walkability-analysis) to provide an overview of connectivity across the entire city  
- Compute **node density** (nodes/km²) as a measure of network connectivity  
- Visualize node distribution and density per municipality  



---

### 2. Accessibility (`15min_score.ipynb`)
- Reproduced the **15-minute City score** methodology as referenced in the literature, which evaluates access to essential amenities within a 15-minute walk.  
- Adapted the approach to suit the local context of Rome and the available data.  
- To ensure a robust classification of municipalities, calculated the **coverage ratio**, the fraction of hexagons with valid cityscore data.  
- Computed the **mean cityscore** only over hexagons with data, then defined a **weighted cityscore** as:  

  `mean_cityscore_weighted = mean_cityscore × coverage_ratio`  

- This weighting accounts for municipalities with incomplete data by proportionally reducing their scores, reflecting actual lower accessibility due to limited pedestrian connectivity or amenity reachability.  
- Calculated an **aggregated score per municipality** based on this weighted cityscore.  
- Visualized the results for clear interpretation of accessibility patterns across Rome’s municipalities.



---

### 3. Morphology (`slope.ipynb`)
- Provide urban context by relating slope values to the morphology of Rome’s municipalities  
- Download and preprocess **Copernicus DEM** (30m resolution)  
- Reproject DEM to **EPSG:25833** for accurate metric slope computation  
- Compute slope in **degrees** using:  
  - `gdaldem slope` from GDAL  
- Use **zonal statistics** to compute average slope per municipality  
- Visualize slope distribution and its impact on walkability  




---

### 4. 🧮 Composite Walkability Index (inside `slope.ipynb`)
- Normalize individual components:
  - **Node density**
  - **mean_cityscore_weighted**
  - **Average slope (inversely weighted)**
- Combine metrics into a final **Walkability Composite Index (WCI)**
- Rank and visualize the results across municipalities

---

## 📌 Notes & Limitations
- Elevation and slope calculations rely on the accuracy of **COP-DEM GLO-30** data and may be affected by resolution.
- Walkability is modeled purely from spatial structure and access — no behavioral or socioeconomic factors are included (e.g. age, safety, comfort).


---

---

## 📌 Results



---


---

## 📁 Data Sources
- 🗺️ **OpenStreetMap** – Road network and POIs
- 🛰️ **Copernicus DEM GLO-30** – Elevation data (WGS84 / EGM2008)
- 🧾 **Comune di Roma** – Municipio boundaries (GeoPackage)

---

## 🧪 Future Improvements
- Use **RichDEM** or **WhiteboxTools** for more advanced terrain metrics
- Incorporate **land use**, **sidewalk quality**, or **real pedestrian travel time**
- Apply model to **neighborhood** scale or replicate in other cities

---

## 📚 References
- **OpenStreetMap contributors** – https://www.openstreetmap.org
- **Copernicus DEM GLO-30** – European Space Agency (ESA): https://spacedata.copernicus.eu
- **GDAL / GDALDEM** – Geospatial Data Abstraction Library: https://gdal.org
- **GeoPandas**, **Rasterio**, **OSMnx**, **scipy.ndimage**, **matplotlib** – Python libraries used
- Comune di Roma – https://www.comune.roma.it (for administrative boundaries)
- Walkability metrics literature:
  - Frank, L. D., Sallis, J. F., Conway, T. L., Chapman, J. E., Saelens, B. E., & Bachman, W. (2006). *Many Pathways from Land Use to Health: Associations between Neighborhood Walkability and Active Transportation, Body Mass Index, and Air Quality*. Journal of the American Planning Association.
  - Witten, K., Pearce, J., & Day, P. (2011). *Neighbourhood destination accessibility index: a GIS tool for measuring infrastructure support for neighbourhood physical activity*. Environment and Planning A.

----

## 📜 License
MIT License – Feel free to reuse or adapt the code with attribution.
