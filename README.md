# 🏙️ WCI – Walkability Composite Index  
### *Francesco Di Massimo*

Urban walkability can be understood and measured in many different ways, which makes the concept inherently multifaceted. A walkable environment might be defined by a dense and connected street network, or by the availability of diverse services within a short walking distance.

However, other factors — such as the **morphological characteristics** of the urban landscape — can also significantly influence both **walkability**, especially in topographically complex cities like Rome.

This project aims to evaluate the **walkability of the city of Rome** by analyzing a set of spatial metrics derived from **OpenStreetMap (OSM)** data and **satellite elevation data** from the **Copernicus Digital Elevation Model (COP-DEM)**. These metrics are then combined into a **composite accessibility index (WCI)** to identify which municipalities of Rome are more — or less — accessible by foot. The results are intended to inform future urban planning and sustainable mobility strategies.

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

### 1. 🧭 Connectivity Analysis (`connectivity.ipynb`)
- Download and preprocess **OSM street network** using `osmnx`
- Project and clip the network to the administrative boundaries of Rome’s **municipi**
- Compute **node density** (nodes/km²) as a measure of network connectivity
- Visualize node distribution and density ranking per municipio

> 📊 **Output**: Choropleth map of node density by municipio + ranking table

---

### 2. 🕒 15-Minute Access Score (`15min_score.ipynb`)
- Collect and preprocess **POIs (Points of Interest)**: schools, healthcare, shops, transit, etc.
- Create a **15-minute walking isochrone** from the street graph (based on travel time at 4.5 km/h)
- For each municipio, calculate the **proportion of POIs accessible within 15 minutes**
- Normalize the score (0–100 scale) across all zones

> 📊 **Output**: Accessibility score maps + POI coverage summary

---

### 3. 🏔️ Slope Analysis (`slope.ipynb`)
- Download and preprocess **Copernicus DEM** (30m resolution)
- Reproject DEM to **EPSG:25833** for accurate metric slope computation
- Compute slope in **degrees** using:
  - A custom Sobel filter method *(if hardware-limited)*, or
  - `gdaldem slope` from GDAL for efficiency
- Use **zonal statistics** to compute average slope per municipio
- Filter out no-data zones (e.g. sea areas)

> 📊 **Output**: Slope maps (hillshade and grayscale) + slope distribution by municipio

---

### 4. 🧮 Composite Walkability Index (`wci_index.ipynb`)
- Normalize individual components:
  - **Node density**
  - **15-minute score**
  - **Average slope (inversely weighted)**
- Combine metrics into a final **Walkability Composite Index (WCI)**
- Rank and visualize the results across municipalities
- Discuss anomalies (e.g. Giustiniana’s high slope vs. accessibility)

> 📊 **Output**: Final WCI map + composite table with ranking + interpretation of results

---

## 📌 Notes & Limitations
- Elevation and slope calculations rely on the accuracy of **COP-DEM GLO-30** data and may be affected by resolution.
- Walkability is modeled purely from spatial structure and access — no behavioral or socioeconomic factors are included (e.g. age, safety, comfort).
- Due to hardware limitations, **RichDEM** was not used; slope was computed with GDAL or Sobel filtering.

---

## 📁 Repository Structure
- **Walkability-Rome/**
  - **data/**  
    Cartella con tutti i dati di input (DEM, esportazioni OSM, confini)
    - **slope_d/**  
      DEM e output della pendenza
    - **connectivity_d/**  
      Rete e nodi processati
    - **poi_d/**  
      Punti di interesse e risultati di accessibilità
  - **notebooks/**  
    Notebook Jupyter con le analisi principali
    - `connectivity.ipynb`  
      Densità nodi e metriche di rete
    - `15min_score.ipynb`  
      Calcolo del punteggio di accessibilità a 15 minuti
    - `slope.ipynb`  
      Analisi della pendenza e metriche del terreno
    - `wci_index.ipynb`  
      Creazione dell’indice composito e risultati finali
  - **outputs/**  
    Grafici, mappe, esportazioni CSV
    - **rankings/**  
      CSV con ranking di densità nodi, pendenza, WCI
    - **maps/**  
      Mappe tematiche, hillshade, mappe indice finale
  - **utils/**  
    Script e funzioni di supporto riutilizzabili
  - `README.md`  
    Documentazione del progetto
  - `requirements.txt`  
    Dipendenze Python

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
