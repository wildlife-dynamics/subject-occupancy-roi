# Subject Occupancy in Regions

Estimates each subject's home range from its EarthRanger GPS tracks, then measures how much of each region (for example a conservancy, park or census block) that home range covers. The results are shown as a map of the home ranges over the regions, plus a table of each subject's occupancy per region.

Documentation: [docs/index.html](docs/index.html) (User Guide, Technical Guide and Troubleshooting).

## What it produces

- **Home Range Map**: each subject's elliptical time-density home range, coloured by percentile (50% to 99.9%), drawn over the chosen regions with EarthRanger's own colours.
- **Occupancy table**: for each subject and each region its home range reaches:

  | Column | Meaning |
  |---|---|
  | Subject Name | The subject |
  | region | The region's name |
  | Region Area (km²) | The whole region's area |
  | Area Covered (km²) | The part of the region inside the subject's home range |
  | Occupancy (%) | Area Covered ÷ Region Area × 100: the share of the region the home range covers |

- **CSV files** of the occupancy table, one per dashboard view.

### How occupancy is measured

- The home range is the subject's **99.9th percentile** elliptical time-density area.
- Areas are measured in metres in the local UTM zone of the home range, so they stay accurate at any latitude.
- Regions with the same name are merged into one region first.
- Only regions with an area are measured. Lines and points, such as roads and outposts, are drawn on the map but left out of the table.
- Regions the home range doesn't reach are left out of the table.
- Regions may overlap, so one subject can cover parts of several regions at once.

## Requirements

- An **EarthRanger** connection set up in the workflow runner.
- A **subject group** in EarthRanger whose subjects have GPS fixes in the time range.
- **Regions** saved in EarthRanger as features (polygons), for example in a feature set such as `Boundaries` or with a feature type such as `Conservancy`.

---

## 1. Load the Workflow

In the workflow runner, add a new template using this repository's URL. Once added, select **subject-occupancy-roi** from the templates list to load it.

---

## 2. Configure the Workflow

### Set Workflow Details

| Field | Description | Default |
|---|---|---|
| Workflow Name *(required)* | A short name to identify this run | — |
| Workflow Description | Optional notes about the run | empty |

### Set Groupers

How the dashboard is split into views.

| Field | Description | Default |
|---|---|---|
| Group by | Add **Category → Subject Name** to give each subject its own view of the map and table. Leave the list empty to show every subject together in one view. | empty (one view) |

Category is limited to **Subject Name**. The form also offers **Time** and **Spatial** grouping, but this workflow doesn't support them yet: leave them out (see Troubleshooting).

### Set Time Range

| Field | Description | Default |
|---|---|---|
| Since *(required)* | Start of the period to analyse | — |
| Until *(required)* | End of the period to analyse | — |
| Timezone | Optional timezone for Since/Until | — |

### Connect to EarthRanger

| Field | Description | Default |
|---|---|---|
| Data source *(required)* | The EarthRanger connection to use | — |

### Basemap URLs

The map imagery under the home ranges and regions. The defaults can be left as they are.

### Regions

The EarthRanger features to measure occupancy in. They are also drawn on the map.

| Field | Description | Default |
|---|---|---|
| Select regions by | How to pick the regions (see below) | Feature sets |

| Option | Field | Description |
|---|---|---|
| Feature sets *(default)* | Feature set names | Every feature in each named set becomes a region, e.g. `Boundaries`. Leave empty to use every feature set on the site. |
| Feature types | Feature type names | Every feature of these types becomes a region, e.g. `Conservancy` or `Park`, whichever set it is in. |
| Feature IDs | Feature IDs | Individual features, by their EarthRanger ID. |

Names must match EarthRanger exactly, including capitals. A wrong name stops the run with a list of the names that exist.

> Leaving **Feature sets** empty fetches every feature on the site, which can include large sets such as roads. Name the sets or types you need to keep runs quick.

### Subject Group Name

| Field | Description | Default |
|---|---|---|
| Subject Group Name *(required)* | The subject group whose home ranges are measured, picked from a drop-down of the data source's subject groups. ⚠️ A group with mixed subtypes could lead to unexpected results. | — |

---

## 3. Run the Workflow and find the results

Click **Submit**. The workflow fetches the subjects' observations and the regions, builds trajectories, estimates home ranges, measures occupancy and draws the dashboard.

Files are written to the workflow's results folder (`ECOSCOPE_WORKFLOWS_RESULTS`):

| File | Description |
|---|---|
| `*.csv` | The occupancy table, one file per dashboard view |
| `*.html` | The map and the table shown on the dashboard |

---

## Developing

```bash
# recompile spec.yaml (first time: --install; after that: --update, which bumps VERSION.yaml)
./dev/recompile.sh --update

# run the test cases in test-cases.yaml
pixi run --manifest-path pixi.toml -e default bash dev/pytest-cli.sh subject_occupancy_roi --all
```

The occupancy, region and styling tasks (`get_features`, `compute_region_occupancy`, `style_spatial_features`) come from `ecoscope-workflows-ext-mep`; see the [Technical Guide](docs/technical-guide.html).
