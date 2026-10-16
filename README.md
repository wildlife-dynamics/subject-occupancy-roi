# subject-occupancy-roi

subject-occupancy-roi

## What it does

| Step | What it does |
|---|---|
| **Set Workflow Details** | Add information that will help to differentiate this workflow from another. |
| **Set Groupers** |  |
| **Set Time Range** | Choose the period of time to analyze. |
| **Connect to EarthRanger** |  |
| **BaseMaps** |  |
| **Get Features** | Fetch EarthRanger features as a GeoDataFrame, chosen by feature set, feature type or feature ID, with each feature's type and set. |
| **Subject Group Name** | Get observations for a subject group from EarthRanger. |
| **Convert Crs** | Re-project a GeoDataFrame's geometries to the given CRS. |
| **Process Relocations** |  |
| **Style Spatial Features** | Style EarthRanger features for the map, the same way as STE's get_spatial_features: native EarthRanger colours first, then any overrides in `style`. Pass the result to create_spatial_features_layer. |
| **Relocations To Trajectory** |  |
| **Create Spatial Features Layer** | Create GeoJsonLayer / IconLayer definitions for one or more styled GDFs. |
| **Split Groups** |  |
| **Calculate Elliptical Time Density Grouped** | Compute elliptical time-density home ranges per group. |
| **Filter Rows** |  |
| **Convert Crs** | Re-project a GeoDataFrame's geometries to the given CRS. |
| **Compute View State From Gdf** |  |
| **Apply Color Map** | Adds a color column to the dataframe based on the categorical values in the specified column. |
| **Compute Region Occupancy** | How much of each region each subject's home range (its `percentile` time density range) covers. |
| **Filter Rows** |  |
| **Create Geojson Layer** | Creates a GeoJSON layer definition based on the provided configuration. |
| **Persist Df** | Persist dataframe to a file or cloud storage object. |
| **Combine Deckgl Map Layers** | Combine static and grouped map layers into a render-ready list. |
| **Map Columns** | Maps and transforms the columns of a DataFrame based on the provided parameters. The order of the operations is as follows: drop columns, retain/reorder columns, and rename columns. |
| **Draw Table** | Creates an HTML table of the provided dataframe. |
| **Groupbykey** |  |
| **Persist Text** | Persist text to a file or cloud storage object. |
| **Draw Map** | Creates a map based on the provided layer definitions and configuration. |
| **Create Table Widget Single View** | Create a table widget with a single view. |
| **Persist Text** | Persist text to a file or cloud storage object. |
| **Merge Widget Views** | Merge widgets with the same `title` and `widget_type`. |
| **Create Map Widget Single View** | Create a map widget with a single view. |
| **Merge Widget Views** | Merge widgets with the same `title` and `widget_type`. |
| **Gather Dashboard** |  |

## Requirements

- Access to a **EarthRanger** data source, set up as a connection

---

## 1. Load the Workflow

In the workflow runner, add a new template using this repository's URL. Once added, select
**subject-occupancy-roi** from the available templates list to load it.

---

## 2. Configure the Workflow

You'll be prompted to fill in the following parameters before running.

### Set Workflow Details

| Field | Description | Default |
|---|---|---|
| Workflow Name *(required)* |  | — |
| Workflow Description |  | `` |

### Set Time Range

| Field | Description | Default |
|---|---|---|
| Since *(required)* | The start time | — |
| Until *(required)* | The end time | — |
| Timezone |  | — |

### Connect to EarthRanger

| Field | Description | Default |
|---|---|---|
| Data source *(required)* | Select one of your configured data sources. | — |

### Subject Group Name

| Field | Description | Default |
|---|---|---|
| Subject Group Name *(required)* | ⚠️ The use of a group with mixed subtypes could lead to unexpected results | — |

---

## Developing

```bash
# recompile spec.yaml (first time: --install; after that: --update, which bumps VERSION.yaml)
./dev/recompile.sh --update

# run the test cases in test-cases.yaml
pixi run --manifest-path pixi.toml -e default bash dev/pytest-cli.sh subject_occupancy_roi --all
```

Documentation: [docs/index.html](docs/index.html).
