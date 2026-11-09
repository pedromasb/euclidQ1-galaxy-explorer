# 🔭 Cluster Member Explorer

Two-page Streamlit app for exploring galaxy cluster members in Euclid Q1, combining
[Zoobot](https://github.com/mwalmsley/zoobot) morphology predictions with Euclid physical
properties.

**Try it in your browser:**
[pedromasbuitrago.com/software/euclidq1-galaxy-explorer](https://www.pedromasbuitrago.com/software/euclidq1-galaxy-explorer/)
— it runs entirely client-side, with nothing to install.

## Pages

- **Cutout Explorer** — filter the catalogue by physical properties (photometric redshift,
  stellar mass, r/r500, local projected density) and Zoobot morphology fractions, then browse
  30 cutouts at a time, sorted by any of those columns. Click a galaxy's coordinates to open
  it in ESASky.
- **Scatter Plot** — plot any two Zoobot columns against each other, optionally coloured by
  a third, and lasso or box-select up to 15 galaxies to see their properties and cutouts.
  Selecting points triggers a rerun, and the selection is shown below the plot.

## Running locally

```bash
pip install -r requirements.txt
streamlit run Home.py
```

Run it from the repository root, since the catalogue is loaded by relative path.

## Data

- **Catalogue** — `catalogue_st.csv`, 4,878 galaxies, loaded by `load_data()` in
  `common.py`. `p_bar` is derived on load as `1 - bar_no_fraction`.
- **Cutouts** — JPGs hosted in the public Hugging Face dataset
  [`pmb17/euclid-cluster-cutouts`](https://huggingface.co/datasets/pmb17/euclid-cluster-cutouts).
  The browser fetches them directly, so nothing has to be served locally. On load, the
  `jpg_loc_gz_arcsinh_vis_y` column is reduced to a bare filename, so catalogues that still
  carry absolute paths work unchanged, and `cutout_url()` builds each image's URL.

## Customising

Everything lives in `common.py`:

- `PHYSICAL_DISPLAY` and `ZOOBOT_DISPLAY` choose which columns appear as filters and scatter
  axes. Each physical filter also sets its slider `step` and display `format`.
- `CUTOUT_BASE_URL` points at the cutout dataset.
- `get_esasky_url()` builds the ESASky link — change the HiPS layer or field of view there.

## File structure

```
Home.py                 ← landing page
common.py               ← configuration, column definitions, data loading, slider helpers
catalogue_st.csv        ← the catalogue
requirements.txt
pages/
  1_Cutout_Explorer.py  ← filters and cutout gallery
  2_Scatter_Plot.py     ← morphology scatter plot and selection
```

## Notes

- Catalogue values are single-precision floats, so a redshift of 0.35 is stored as
  0.34999999. `slider_bounds()` snaps each slider's range onto its step grid, and `in_range()`
  compares with a tolerance far below one step. Without them, dragging a slider's upper handle
  did not update the page, and filtering at a bin edge silently dropped that whole bin.
- The gallery is capped at 30 cutouts to keep the page responsive.
