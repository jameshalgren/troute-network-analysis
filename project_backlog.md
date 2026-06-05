# Project Backlog & Background: troute-network-analysis

## Background

The National Water Model (NWM) models the stream channels in the United States as a directed acyclic graph (DAG) network. The core mapping of this network is defined in the `RouteLink` dataset (e.g., `RouteLink_CONUS.nc`). Within this massive national network, there are approximately 12,000 independent subnetworks. Each subnetwork is a collection of channel segments starting from headwaters and coalescing down to a single shared terminal outlet (such as an ocean outlet or an inland sink).

While executing t-route with the full network is remarkably efficent, there are a number of reasons to subset the full network for more detailed diagnostic analysis of NWM outputs. The `troute-network-analysis` repository provides tools for exploring subnetworks of the NWM routing network and, in particular, to provide a simple method for preparing mask files which may be used to reduce the size of the routing domain within `t-route` application.

While the initial demo notebook (`Network_Analysis_via_Parallelization_Demo.ipynb`) focuses on parallelization of these independent networks, the repository's primary role moving forward is supporting subnetwork subsetting, diagnostic queries, visualization, and test package generation for `t-route` model runs.

---

## Task Backlog

Here is the prioritized list of tasks to implement in this repository:

### 1. Extract Clean Upstream Reach Masks (`get_upstream_mask()`)
- **Objective**: Currently, getting a list of upstream reach IDs for a given segment requires filtering through verbose print outputs and duplicates. We need a clean utility function, `get_upstream_mask(terminal_key, connections, ...)` that returns a simple Python list or set of unique reach IDs.
- **Goal**: Allow users to easily generate a mask file for direct input into a `t-route` model execution without manual text editing or cleanup.

### 2. Expand README.md with Scientific/Operational Context
- **Objective**: Enhance the top-level repository [README.md](file:///Users/halgren/git/troute-network-analysis/README.md) with details about:
  - The NWM network topology model.
  - The structure of the `RouteLink` NetCDF file.
  - How subnetworks are defined and why subsetting is valuable for diagnostic runs.

### 3. Build USGS Gage to RouteLink LinkID Mapping Database
- **Objective**: Create a reference file mapping USGS Gage IDs to NWM RouteLink `linkid` values.
- **Details**: 
  - Use the `gage` field in the `RouteLink` dataset, which contains USGS Gage codes.
  - The network above any gage can then be selected using the gage's corresponding `linkid`.
  - Specifically include and map the three locations already featured in the notebook:
    - **Mulberry Creek at Jones, AL**: USGS Gage ID `02422500`
    - **Walnut Creek above Clanton, AL**: USGS Gage ID `02408150`
    - **Cahaba River at Centreville, AL**: USGS Gage ID `02424000`

### 4. Improve Subgraph ASCII Tree Visualizations
- **Objective**: Provide clear, step-by-step demonstrations in the notebooks showing how to generate ASCII tree diagrams for arbitrary selected subgraphs.
- **Details**: Right now, it is not obvious how to use the existing `recursive_print` utilities specifically for subset subgraphs like Mulberry Creek, Walnut Creek, or Cahaba River.

### 5. Implement Geographic Mapping Functions
- **Objective**: Integrate geographic visualization tools (e.g., using `geopandas` and `matplotlib` or `folium`) to plot selected subnetworks on a map.
- **Details**: Support plotting shapefile-based segments of subsetted basins (like Brazos or custom basin subgraphs) dynamically in the notebook.

### 6. Generate Test Input Packages for T-Route Simulation
- **Objective**: Establish a workflow to build a complete test input package for a specific date and subsetted subnetwork.
- **Details**: This will allow testing operational simulations in `t-route` using identical inputs and routing functions to the National Water Model, but restricted to the compact subnetwork.
