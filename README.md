# EEA-AQA

# Air Quality Analysis (AQA) Library

Welcome to the **Air Quality Analysis (AQA) Library**, an integrated framework designed to automate the download, processing, and harmonization of environmental data from multiple sources.  
This project enables the combination of **European Environment Agency (EEA) ground data**, **Sentinel-5P satellite observations**, and **ERA5 reanalysis** variables to generate daily, spatially consistent air quality indicators.

---

## Data Requirements

To execute this notebook successfully, you will need access to the following datasets:

- **EEA (Ground Sensors)**  
  Daily, Hourly, Monthly, or Yearly pollutant concentration measurements (NO₂, SO₂, CO), including `idsensore`, `lat`, `lng`, `provincia`, and timestamp columns.

- **Sentinel-5P (Satellite)**  
  Atmospheric column densities for selected pollutants (`NO2_column_number_density`, `CO_column_number_density`, etc.) accessed through the **Google Earth Engine API**.

- **ERA5 (Reanalysis)**  
  Meteorological parameters (temperature, pressure, wind speed, boundary layer height, radiation, precipitation, etc.) obtained from **CMCC** or **Copernicus Climate Data Store (CDS)**.

---

## Features

- **Automated download** of ERA5, Sentinel-5P, and EEA datasets.  
- **Data cleaning** and normalization for all input sources.  
- **Temporal harmonization** to 12:00–15:00 mean values.  
- **Spatial merging** of satellite, reanalysis, and sensor data.  
- **Integrated summary table** for pollutant concentrations and weather variables.  
- **Visualization tools** for pollutant maps and correlation analysis.

---

## Installation

### Option 1 — Conda 
If using Windows: environment.yml
If using Mac: nobuilds.yml
```bash
conda env create -f environment.yml
conda activate AQA_DayRange
```

### Option 2 — Pip
```bash
python -m venv .venv
.venv\Scripts\activate   # Windows
source .venv/bin/activate  # macOS/Linux
pip install -r requirements.txt
```

---

## Code

All functions and workflow are implemented in the **Jupyter Notebook**:
```
EEA_AQA.ipynb
```

This notebook contains:
- EEA data access and cleaning.
- ERA5 and Sentinel-5P integration.
- Spatial grid generation and interpolation.
- Computation of current (`curr_`) and previous (`prev_`) daily means.
- Export of harmonized results to CSV and visualization of pollutant maps.

---

## Workflow Overview

### 1. **EEA Data Processing**
Loads ARPA Lombardia datasets using the API and organizes metadata for sensors, pollutants, and coordinates.


---

### 2. **ERA5 Variable Extraction**
Downloads meteorological variables (temperature, pressure, wind speed, radiation, BLH, etc.) using CMCC or CDS API and converts them to a harmonized format.

#### Example:
```python
import cdsapi
c = cdsapi.Client()
c.retrieve('reanalysis-era5-single-levels', {...}, 'era5_data.nc')
```

---

### 3. **Sentinel-5P Pollutant Integration**
Retrieves pollutant column density data (e.g., NO₂, O₃, CO, SO₂) via **Google Earth Engine** and scales values for consistency with ground units.

#### Example:
```python
pollutants = {
    "no2": {"collection": "COPERNICUS/S5P/OFFL/L3_NO2", "band": "NO2_column_number_density"}
}
```

---

### 4. **Spatial Harmonization and Merging**
Combines all datasets (EEA, ERA5, Sentinel) through coordinate matching and averaging based on the AOI grid.

---

## Outputs

- **CSV results**
- **Summary tables**: harmonized pollutant and meteorological data  

---

## Repository Structure

```
EEA_AQA/
│
├── EEA_AQA.ipynb              # Main analysis notebook
├── EEA_Metadata_Clean.ipynb   # Metada cleaning and preprocessing for further use in the pipelin (creating a clean dataframe)
├── eea_sensor_meta.csv        # CSV file for stations metadata from EEA
├── eea_sensor_meta.parquet    # PARQUET file for stations metadata from EEA
├── environment.yml            # Conda environment for windows
├── nobuilds.yml               # Conda environment for Mac
├── requirements.txt           # pip dependencies
└── README.md                  # Project documentation
```

---

## Technologies Used
- **Python** (pandas, geopandas, numpy, matplotlib, xarray, requests)
- **Google Earth Engine API**
- **Copernicus CDS / CMCC ERA5**
- **EEA Open Data API**
- **GeoPandas + Matplotlib** for geospatial analysis

---

## Testing
The data pipeline has been validated across multiple pollutants and date ranges.  

---

## License
This project is licensed under the **MIT License**.  
See the [LICENSE](LICENSE) file for details.

---

## Author
**Claudia Isabela Saud-Miño**  
Politecnico di Milano — Geoinformatics Engineering  
📧 [Contact via GitHub](https://github.com/Saudisis)
