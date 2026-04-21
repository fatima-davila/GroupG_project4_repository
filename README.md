# GroupG_project4_repository
Our project 4 files for CIVI 202
In this project Risk Averse, LLC has charged our team to perform a sensitivity analysis to evaluate the potential of bias within FEMA’s National Risk Index (NRI) scoring methods. In the NRI, FEMA defines risk as the product of the probability of a natural disaster and the expected outcome and damage of the natural disaster scenario. This rating system for risk is percentile score for individual communities ranks them from “Very Low” to “Very High” based on their percentile ranking compared to other communities at the same level of risk. Withing these ratings, the communities also receive ratings based on Expected Annual Loss, Social Vulnerability, and Community Resilience.  

In this repository you will find the final report with all of our findings, the gantt chart listing all the tasks executed, the timesheet, the scope of work, and the all the files that will be needed to run the code. 

#Below are instructions on how to use the jupyter notebook
## Objectives
- Import and prepare NRI shapefile data and state SVI CSV files.
- Filter the national dataset to Iowa and South Dakota census tracts.
- Merge NRI and SVI data using census tract FIPS codes.
- Clean missing numeric values using median imputation.
- Create a proposed population-based risk metric for tornado and hail hazards.
- Compare FEMA NRI tornado risk results with the custom risk metric through maps.
- Visualize differences in spatial risk patterns across the two states.

## Proposed Risk Definition
The proposed tornado risk metric is defined as:

```python
risk_level = (hazard_annualized_frequency * expected_annual_loss_buildings) / population
```

For tornado risk, this becomes:

```python
(TRND_AFREQ * TRND_EALB) / POPULATION
```

This metric estimates a per-person hazard burden by scaling expected losses and event frequency by the number of people in each census tract.

## Input Files
Make sure the following files are in the same working directory as the notebook before running it:

- `NRI_Shapefile_CensusTracts.shp` and associated shapefile support files
- `Iowa (1).csv`
- `SouthDakota.csv`

## Required Python PackagesMake sure the following files are in the same working directory as the notebook before running it:

- `NRI_Shapefile_CensusTracts.shp` and associated shapefile support files
- `Iowa (1).csv`
- `SouthDakota.csv`

## Required Python Packages
Install these packages before running the notebook:

```python
import pandas as pd
import geopandas as gpd
import matplotlib.pyplot as plt
import contextily as ctx
import seaborn as sns
```

You may need to install missing packages with pip, for example:

```bash
pip install pandas geopandas matplotlib contextily seaborn
```

## Workflow Summary
1. Import the required Python libraries.
2. Read the FEMA NRI census tract shapefile with GeoPandas.
3. Read Iowa and South Dakota SVI CSV files with pandas.
4. Convert tract identifiers to 11-digit strings so the datasets can be merged correctly.
5. Filter the NRI shapefile to Iowa and South Dakota.
6. Merge each state's NRI data with its corresponding SVI data using the `TRACT` field.
7. Reproject the merged GeoDataFrames to `EPSG:3857` for mapping.
8. Fill missing numeric values using each dataframe's median.
9. Create custom tornado and hail risk columns.
10. Plot maps showing FEMA NRI tornado risk and the custom population-based tornado risk.

## Main Outputs
The notebook generates map visualizations for:

- Iowa tornado risk using FEMA NRI (`TRND_RISKS`)
- Iowa tornado risk using the proposed population-based score (`risk_lvl_tnd_ia`)
- South Dakota tornado risk using FEMA NRI (`TRND_RISKS`)
- South Dakota tornado risk using the proposed population-based score (`risk_lvl_tnd_sd`)

Additional hail risk calculations are also included:

- `risk_lvl_hl_ia`
- `risk_lvl_hl_sd`

## Notes About the Code
- Missing numeric data are replaced with the median value of each numeric column within the same state's merged dataframe.
- The notebook includes a reusable `plot_map()` function to simplify choropleth mapping.
- The workflow is designed for tract-level comparison rather than county- or state-level aggregation.

## Important Corrections to Check Before Final Submission
Review these items in the notebook before turning it in:

1. The South Dakota merge should use `sd_svi`, not `ia_svi`.
2. Check for any stray characters at the end of cells, such as an extra `Z` after the final plotting command.
3. Make sure all map titles, captions, and variable descriptions match the final version of your analysis.

A corrected South Dakota merge should look like this:

```python
sd_merged = sd_nri.merge(
    sd_svi,
    on="TRACT",
    how="left",
    suffixes=("_nri", "_svi")
)
```

## Interpretation
FEMA's NRI tornado risk score reflects broader community-level consequence by considering expected annual loss together with social vulnerability and resilience. The proposed risk definition instead emphasizes the burden relative to population, which can make smaller communities appear more at risk when losses are large on a per-person basis. Using both methods together can provide a more complete understanding of disaster vulnerability and may help support future decisions about hazard mitigation and FEMA funding.
