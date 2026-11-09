# 🔭 Cluster Member Explorer

Two-page Streamlit app for exploring galaxy cluster members with Zoobot morphology and Euclid physical properties.

## Quick start

```bash
pip install -r requirements.txt
streamlit run Home.py
```

## Setup

1. **Place your catalogue** as `cluster_members.csv` in this directory (or edit `CATALOGUE_PATH` in `common.py`).

2. **Cutout paths**: the app reads the `jpg_loc_gz_arcsinh_vis_y` column for the local path to each JPG. Make sure these paths are accessible from where you run Streamlit. If they are relative paths, run `streamlit run Home.py` from the appropriate working directory.

3. **Customise**: edit `common.py` to change which columns appear as filters, scatter-plot axes, or to adjust the ESASky HiPS layer.

## File structure

```
Home.py                 ← landing page
common.py               ← shared config, column definitions, data loading
requirements.txt
.streamlit/config.toml  ← enables static file serving
pages/
  1_Gallery_Explorer.py ← Page 1: cutout gallery with filters
  2_Scatter_Plot.py     ← Page 2: interactive scatter + selection
```

## Notes

- **p_bar** is computed as `1 - bar_no_fraction` and added as a derived column on load.
- The Gallery page serves images as inline base64 to avoid static-file routing issues. For very large catalogues (>10k rows visible at once) this may be slow; the 30-cutout cap keeps it snappy.
- The Scatter page uses Plotly's built-in lasso/box select. Selected points trigger a `rerun` and their properties + cutouts are displayed below the plot.
