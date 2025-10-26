# 🛰️ NextEra Energy Drone Optimization Challenge

## 🧭 1. Problem Overview

NextEra Energy operates autonomous drones to inspect tens of thousands of miles of power lines. The challenge is to **plan efficient drone inspection missions** that cover all required photo waypoints while staying within the battery limit and returning to the depot after each mission.

The objective is to **minimize total cost** (distance/time) while ensuring:

* Every photo waypoint is visited at least once.
* Each mission stays within the **37,725 ft** battery limit.
* All flights remain within the allowed airspace polygon.

---

## 📁 2. Data Files Description

| File                  | Type                | Description                                                                                                                                       |
| --------------------- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| `distance_matrix.npy` | NumPy array (N x N) | Symmetric matrix of shortest flight distances between all waypoint pairs inside the allowed flight zone. Used as the cost matrix in optimization. |
| `predecessors.npy`    | NumPy array (N x N) | Predecessor indices for reconstructing shortest paths between waypoints using Dijkstra conventions.                                               |
| `points_lat_long.npy` | NumPy array (N x 2) | Longitude and latitude coordinates for each waypoint index. Used for visualization and mapping.                                                   |
| `asset_indexes.npy`   | NumPy array         | Range of waypoint indices representing electrical assets (poles).                                                                                 |
| `photo_indexes.npy`   | NumPy array         | Range of waypoint indices representing photo inspection points for each asset.                                                                    |
| `polygon_lon_lat.wkt` | Text (WKT)          | Polygon geometry defining the allowed flight region. Can be loaded with Shapely or GeoPandas for visualization and validation.                    |

> **Note:** These datasets were provided by the organizers and are **not included** in this repository due to confidentiality.

---

## ⚙️ 3. Our Approach

We began by analyzing the problem and established that it belongs to the class of **NP-hard problems**, meaning that finding an exact optimal solution would require **exponential time (E^E computations)** as the number of waypoints grows.

Our exploration included:

* **Minimum Spanning Tree (MST):** Considered as a heuristic but discarded because it does not ensure valid traversal paths and can lead to suboptimal routing.
* **Google OR-Tools:** Adopted for its efficient heuristics and constraint handling capabilities for Vehicle Routing Problems (VRP).
* **Cost Function Optimization:** We experimented with combining **distance** and **time** as cost factors to find better trade-offs and encourage the optimizer to utilize multiple vehicles (missions).

### Pipeline Summary

1. **Data Inspection:** Load and validate datasets (`numpy.load`).
2. **Unconstrained TSP:** Generate an initial route using OR-Tools.
3. **Path Reconstruction:** Decode indirect routes using `predecessors.npy` to ensure polygon adherence.
4. **Mission Segmentation:** Split routes into valid missions under the 37,725 ft battery limit.
5. **Visualization:** Map the planned routes using Plotly/GeoPandas to confirm coverage.
6. **Evaluation:** Compute total distance, mission count, and verify that all photo points were visited.

---

## 🔒 4. Data Confidentiality Notice

We have **not uploaded any of the original data files** (`.npy` and `.wkt` files) provided by NextEra Energy for this challenge, as they contain **sensitive and proprietary information**.
This repository only includes our **code**, **visualizations**, and **methodology** to reproduce the pipeline structure (not the actual dataset).

---

## 🧩 5. References

* [Google OR-Tools Routing Library](https://developers.google.com/optimization/routing)
* [SciPy Dijkstra Documentation](https://docs.scipy.org/doc/scipy/reference/generated/scipy.sparse.csgraph.dijkstra.html)
* [Plotly Mapbox](https://plotly.com/python/scattermapbox/)
* [Shapely / GeoPandas](https://geopandas.org/)

---
