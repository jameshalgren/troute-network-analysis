# troute-network

An independent Python package for NHD channel network traversal, analysis, and parallelized routing preparation. This code was extracted from NOAA's `t-route` application to allow isolated exploration, optimization, and reuse.

## Features

- **Network Traversal**: Traverses DAG-structured stream networks to find upstream/downstream connections, headwaters, junctions, and reaches.
- **DAG Characterization**: Counts segments, reaches, and junctions; isolates independent subnetworks.
- **ASCII Tree Printing**: Visualizes network hierarchies in plain text.
- **Parallelization Utilities**: Prepares subnetwork routing computations for system-level and network-level multi-processing.
- **Data Loaders**: Reads shapefiles (via `geopandas`/`fiona`), NetCDFs (via `xarray`/`netcdf4`), and compressed CSVs.

## Installation

To install this package locally in editable mode:

```bash
pip install -e .
```

Ensure you have your virtual environment active before installing.

## Package Structure

```
troute-network-analysis/
├── pyproject.toml              # Build & dependency metadata
├── README.md                   # Package documentation
├── src/
│   └── troute_network/
│       ├── __init__.py         # Public API exposure
│       ├── networkbuilder.py   # Downstream & upstream connection building logic
│       ├── nhd_network_utilities.py # File readers & configuration for supernetworks
│       ├── recursive_print.py  # ASCII network visualizer & printer
│       └── network_dl.py       # Large network downloader utility
├── notebooks/
│   └── Network_Analysis_via_Parallelization_Demo.ipynb # Interactive exploration notebook
└── test_data/                  # Demo test shapefiles (Brazos Basin subset)
```

## Running the Demo Notebook

A complete demo notebook is located in the `notebooks` directory. It uses a subset of the Brazos & Lower Colorado River networks.

To run the notebook:
1. Install this package in your environment: `pip install -e .`
2. Install Jupyter or VS Code Jupyter extension.
3. Open `notebooks/Network_Analysis_via_Parallelization_Demo.ipynb`.
4. Run the cells. It will load data from `test_data/Channels/NHD_BrazosLowerColorado_Channels.shp`.

*Note: For the full CONUS networks (such as `CONUS_ge5` or `CONUS_FULL_RES_v20`), you must supply the large ~269 MB RouteLink NetCDF file (`RouteLink_CONUS.nwm.v3.0.20.nc`). This file is omitted from this repository due to size but may be obtained from the NWS/NCO parameter download site here: https://www.nco.ncep.noaa.gov/pmb/codes/nwprod/
Note that the link will occasionally update as the model versions are incremented; the current file is found at this link: https://www.nco.ncep.noaa.gov/pmb/codes/nwprod/nwm.v3.0.20/parm/domain/RouteLink_CONUS.nc
Note also the slight name change in the file as expected for the notebook and scripts in this package.*
