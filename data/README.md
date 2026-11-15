# Data

- `raw/`: small source files, kept exactly as downloaded. In the repository.
- `processed/`: county-level tables produced by the notebooks. In the repository.
- `large/`: large downloads and files derived from them. **Not in the repository** (excluded by `.gitignore`); download them as described below.

## Large files to download

Put each download in the folder shown. The notebooks expect these exact names.

| Dataset | What to download | Put it in | Size | Used by |
|---|---|---|---|---|
| BTS National Transportation Noise Map | Under **2022/2023**: *Continental U.S. road, rail, and aviation noise*. Unzip it. | `large/bts_2022_combined/CONUS_rail_road_and_aviation_noise_2022/` | ~27 GB | 03, 04 |
| 2020 Census blocks, Ohio | TIGER/Line Shapefiles → **2022** → Blocks (2020) → Ohio (`tl_2022_39_tabblock20`). Unzip it. | `large/census_blocks/tl_2022_39_tabblock20/` | ~0.3 GB | 03, 04, 05 |
| Microsoft US Building Footprints | `Ohio.geojson.zip`. Unzip it. | `large/buildings/Ohio.geojson` | 1.5 GB | 04 |
| ACAG PM2.5, V6.GL.02.04 | **NA → Annual**, the five files for 2018–2022 (`V6GL02.04.CNNPM25.NA.201801-201812.nc` and so on) | `large/pm25/` | ~0.5 GB | 06 |
| Census county boundaries | Cartographic boundary files → 2022 → Counties, 1 : 500,000 (national) shapefile. Leave it zipped. | `large/boundaries/cb_2022_us_county_500k.zip` | 11 MB | 07, 09 |

Download pages:

- BTS noise map: <https://www.bts.gov/geospatial/national-transportation-noise-map>
- TIGER/Line shapefiles: <https://www.census.gov/geographies/mapping-files/time-series/geo/tiger-line-file.html>
- Microsoft building footprints: <https://github.com/microsoft/USBuildingFootprints>
- ACAG PM2.5: <https://sites.wustl.edu/acag/surface-pm2-5/>
- Cartographic boundary files: <https://www.census.gov/geographies/mapping-files/time-series/geo/cartographic-boundary.html>

Notes:

- **BTS noise map.** The 2022/2023 download has no separate state files. All state rasters are inside one Esri file geodatabase, `CONUS_rail_road_aviation_noise.Overviews/CONUS_rail_road_aviation_noise.gdb`; the Ohio layer is `OH_rail_road_aviation_noise`, which `rasterio` reads directly. The `Ov_i32_L*.tif` files in the same folder are low-resolution display overviews and are not used.
- **Census blocks.** Use the 2022 release of the 2020 blocks: it includes each block's 2020 population (`POP20`).
- **PM2.5.** Take all five years from the same version, as the ACAG README recommends.

## Files created in `large/` by the notebooks

| File | Created by | Contents |
|---|---|---|
| `large/buildings/ohio_building_points.parquet` | 04 | Center point and footprint area of every Ohio building |
| `large/buildings/ohio_people_points.parquet` | 04 | Population points (buildings with their share of block population), reused by 06 |

## Files in `raw/`

| File | Source | How it was obtained |
|---|---|---|
| `A_ohio_county_2018_2022.txt` | CDC WONDER | Deaths by county |
| `B_ohio_county_year.txt` | CDC WONDER | Deaths by county and year |
| `C_ohio_county_sex.txt` | CDC WONDER | Deaths by county and sex |
| `D_ohio_age_2018_2022.txt` | CDC WONDER | Deaths by ten-year age group, statewide |
| `places_2025_county_OH.csv` | CDC PLACES county data, 2025 release | Downloaded by notebook 02 from the data.cdc.gov API |
| `acs5_2022_county_OH.csv` | ACS 2018–2022 5-year | Downloaded by notebook 05 from the Census API |
| `acs5_2022_county_age_OH.csv` | ACS 2018–2022 5-year, table B01001 | Downloaded by notebook 07 from the Census API |

**CDC WONDER queries.** All four use the *Underlying Cause of Death, 2018–2024, Single Race* database at <https://wonder.cdc.gov>, with Ohio as the location, years 2018–2022, and ICD-10 codes I00–I99, grouped as shown in the table. *Export Results* and *Show Zero Values* were checked. WONDER saves tab-separated text with an `.xls` extension; the files were renamed to `.txt`.

The two Census API downloads need a free API key in a `.env` file at the project root (see the main README).
