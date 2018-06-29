# cp-sca-housing
SQL and processing steps used to create housing inputs to SCA's Housing Pipeline

## Table of Contents
- Introduction
- Data sources
- Limitations and future improvements
- Processing steps

## Introduction
- Information on future housing starts is used by the School Construction Authority (SCA) in planning for new schools in NYC. This data is generated annually using information on known and possible housing starts based on currently available project information collected from DOB, HPD, EDC, and DCP
- **Disclaimer** - This information does NOT represent a housing projection produced by DCP, nor can DCP attest to the certainty that each of these developments will lead to future housing starts

## Data sources
### Primary data sources
- **[DCP Housing Developments Database](https://github.com/NYCPlanning/db-housingdev)** - This database is created by DCP using DOB permit and certificate of occupany data
- **HPD projects** - This data is requested from HPD. It contains information on HPD's New Construction projects and includes all projects completed within past 2 years, in construction, or projected to close financing within next 2 years
- **EDC projects** - This data is requested from EDC. It contains information on EDC's pipeline projects that contain residential developments
- **DCP projects** - This data is generated from DCP's internal project tracking system. Several processing steps are required to identify projects that facilitate residential development (detailed in [1a_dcp_data_prep](https://github.com/mqli322/cp-sca-housing/blob/master/1a_dob_data_prep.sql))
- **Cityled areawide rezonings** - This data is manually compiled as DCP lacked a project tracking system prior to 2012. It contains information on areawide rezonings undertaken by NYC that created additional capacity for residential development

### Prerequisites
- Obtain from HPD their annual submission for SCA's Housing Pipeline
- Obtain from EDC their annual submission of pipeline projects for SCA's Housing Pipeline
- Download from DCP's internal project tracking system (ZAP)
  * Project data
  * Project actions
- Download **[NYC Zoning Map Amendments](https://www1.nyc.gov/site/planning/data-maps/open-data/dwn-gis-zoning.page)** - This dataset contains project area polygons for all certified or approved projects seeking a zoning map amendment (ZM action)
- Obtain DCP's imPACT Visualization polygons - This dataset contains the polygons associated with all DCP projects. Because there are accuracy concerns with this dataset, nyzma was used where possible
- Obtain tracker of projected units and build year for areawide rezonings prior to 2012

## Limitations and future improvements
- **Limitations**
  * (exercise disretion when deciding how to use inputs bc of varying degrees of uncertainty)
  * (deduping not perfect)
- **Future improvements**
  * (accurate project area)
  * (cleaner DCP data)
  * (areawide upzoned lots only)
  * (additional analyses - timeframe thresholds, pct of permit apps that are complete, accuracy of areawide projections)
  * (tracking if project went commercial)

## Processing steps
| Step  | Description |
| :--- | :--- |
| [1a_dob data prep](https://github.com/mqli322/cp-sca-housing/blob/master/1a_dob_data_prep.sql) | Filter [DCP Housing Development Database](https://github.com/NYCPlanning/db-housingdev) to relevant residential jobs |
| [1b_hpd data prep](https://github.com/mqli322/cp-sca-housing/blob/master/1b_hpd_data_prep.sql) | Add points representing project address |
| [1c_edc data prep](https://github.com/mqli322/cp-sca-housing/blob/master/1c_edc_data_prep.sql) | Add polygons representing project area|
| [2a_dcp_data_prep](https://github.com/mqli322/cp-sca-housing/blob/master/2a_dcp_data_prep.sql) | Find discretionary actions that facilitate residential units |
| [2b_dcp_geocode](https://github.com/mqli322/cp-sca-housing/blob/master/2b_dcp_geocode.sql) | Add polygons representing project area |
| [2c_dcp_geocode_manual](https://github.com/mqli322/cp-sca-housing/edit/master/2c_dcp_geocode_manual.sql) | Manually add polygons if not captured (or not accurately captured) in existing sources |

