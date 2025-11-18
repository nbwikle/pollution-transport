# A pollution transport model with inferred dynamics and source attribution
*Authors: Nathan B. Wikle and Marcin Jurek*

The following instructions provide details on how to run the source code underlying the analysis, including replication of the main figures and results.

## Requirements

The code has been tested with R version X.  The following **R packages** must be installed before the code will run successfully **(version in parentheses)**:

- [`dplyr`](https://CRAN.R-project.org/package=dplyr)  (version)
- additional packages

## Data

Data have been archived for download at the beginning of the analysis (doi = X). They require X GB of space within the subdirectory. There are **X main data sources:**

### Coal-fired power plant emissions

Emissions data are from the US Environmental Protection Agency's [Clean Air Markets Program Data (AMPD)](https://campd.epa.gov/). The AMPD provides access to current and historical monthly emissions data on electricity generating units (EGUs), collected as part of EPA's emissions trading programs. The downloaded AMPD are named `AMPD_Unit_with_Sulfur_Content_and_Regulations_with_Facility_Attributes.csv`. This file contains monthly emissions totals of SO2 (and other chemical emissions), as well as characteristics of the EGUs from 1995-2017. The following columns are most relevant to our analysis:

- `Facility.ID`: unique six-digit facility identification number, also called an ORISPL, assigned by the Energy Information Administration
- `Unit.ID`: unique identifier for each unit at a facility
- `Year`: the calendar year during which activity occurred
- `Month`: the month in which activity occurred
- `Facility.Longitude`: the physical longitude of the facility
- `Facility.Latitutde`: the physical latitude of the facility
- `SO2..tons`: sulfur dioxide (SO2) emissions, in short tons
- `Has.SO2.Scrub`: denotes use of flue-gas desulfurization (FGD) emissions reduction technologies (i.e., a scrubber is in use) 

Additional variables are not relevant to this analysis, however, a comprehensive list of AMPD column definitions is included for reference (see the  `AMPD_column_definitions.csv` file). 

### Monthly sulfate concentrations

Annual mean sulfate concentrations were obtained from the Randall Martin Atmospheric Composition Analysis Group's [North American Regional Estimates (V4.NA.03) dataset](https://sites.wustl.edu/acag/datasets/surface-pm2-5/#V4.NA.03). The data consist of raster annual mean SO4 data (micrograms per cubic meter; grid resolution = 0.01 x 0.01 degrees), as described in [van Donkelaar et al. (2019)](https://pubs.acs.org/doi/10.1021/acs.est.8b06392). The downloaded SO4 file is named `GWRwSPEC_SO4_NA_201101_201112.nc`. 

### Meteorological data

Meteorological data are from the NOAA Physical Science Laboratory [NCEP North American Regional Reanalysis (NARR)](https://psl.noaa.gov/data/gridded/data.narr.monolevel.html) database. Downloaded files include:
- `uwnd.10m.mon.mean.nc`: monthly mean U-wind at 10 m
- `vwnd.10m.mon.mean.nc`: monthly mean V-wind at 10 m
These files are read into R as raster layers.

### Covariate data

Additional covariate data may be useful when inferring latent sources of SO2. Initially, this will include landcover data and population density.

### Data Preprocessing

Downloaded data are processed at the beginning of this analysis, using `make-facility-data.R` and `data-cleaning.R`. These two files produce a single R list object, saved as `input-data.RDS`, which contains:

1. `so4`: raster of SO4 surfaces
2. `wind`: rasters of wind vector elements
3. `em`: power plant facility emissions data
4. `X`: vector of annual SO2 emissions, matched to SO4 raster elements
5. `pop`: raster of 2010 US population density 
6. `lc`: raster with landcover covariates

## Instructions

Before running any code, make sure the required R packages have been installed. Open and run the `main.R` file, found in the `./src/` folder.  

### Step One: 

- Loads required packages into R.
- Time: < 0.01 seconds.

### Step Two: 

- Creates `./data/` and `./output/` subdirectories, which will hold the underlying data and analysis output, respectively.
- Time: < 0.01 seconds.

### Step Three:

- Downloads the raw data sources used in the analysis. These data are publicly available, and have been archived (doi = X) for reproducibility. These data require X GB of space.
- Time: ~Y minutes.
- Output size: X GB.

### Step Four: 

- Adds all functions from the script, `functions.R`, found in the `./src/` folder.
- Time: < 0.01 seconds.

### Step Five: 

- Loads the raw coal-fired power plant facilities data, cleans the data, and creates a data frame with relevant covariate values. After step three, the raw facility data are stored in the `./data/` folder as `AMPD_Unit_with_Sulfur_Content_and_Regulations_with_Facility_Attributes.csv`. This section saves four output RDS files - `MonthlyUnitData.RDS`, `AnnualUnitData.RDS`, `MonthlyFacilityData.RDS`, and `AnnualFacilityData.RDS` - in the `./data/` folder. 
- Time: ~30 seconds.
- Output size: X MB.

### Step Six: 

- Cleans all data (including environmental covariates, the SO4 response variable, and facilities data). The raster objects are saved as `./data/usa-data.RDS`. This raster contains all data needed for the remaining analysis. The relevant raw data sources can be found in the subfolder, `./data/`, created in Step Two (see Data section above for more details).
- Time: ~Y minutes.
- Output size: X MB.

