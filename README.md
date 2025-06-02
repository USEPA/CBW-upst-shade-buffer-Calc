# CBW-cont-stream-temp-QC

This project details the approach and provies the R code used for the aggregation, reorganization, and quality control (QC) review of continuous stream temperature data from the Chesapeake Bay Watershed (CBW) that were compiled from multiple sources. This example focuses on generating monthly stream temperature statistics for the development of a regional stream temperature model based on the National Hydrography Dataset Plus High-Resolution (NHDPlusHR) flowline network. 

The main branch of this github repository includes the code that was used to generate the final QC-aggregated stream temperature datasets for the CBW and only an example dataset (test_datasets.RData), which can used to run through the QC criteria checks starting with the "Index Stream Water Temperature Dataset for QAQC Criteria" section. All data files and the final stream temperature datasets can be found here [insert link to Zenodo data repository]. A R Markdown .html file is provide for easier viewership. 

The R script is organized into 7 sections:
1) Setup. Includes the installation and loading of R libraries required to run the script.
2) Aggregate Continuous Stream Temperature Data and Site Information. Includes the steps to import, standardize, and aggregate site information and continuous stream temperatue data from USGS NWIS (using dataRetrieval) and multiple individual source files.
3) Initial Cleanup of Repeated Data. Includes initial steps taken to QC the aggregated site and temperature files for repeated (duplicate) measurements under different source/site names, instances where different site names have overlapping coordinates, and instances where coordinates are repeated for different site names. The step was used to identify solutions that were addressed in other areas of the script to avoid overrepresentation of the data prior to performing additional QC checks on the continuous stream temperature data.
4) Initial Subset of Continuous Data. In a separate step, sites and coordiantes were snapped to the NHDPlusHR flowline network using the HydroAdd tool, which needed to be combined with the aggreated site informatin file created with this script. This section details the steps taken to ensure that snapped coordinates were correctly matched with sites aggregated herein where possible. In addition, some temperature data included in the source files is not desired (e.g., from lakes/reservoirs or with unnacceptable data quality indicators) and these sites and temperature data were removed in this section. Code in this section was used to address possible overrepresentation of data detailed in 3).
5) Index Stream Water Temperature Dataset for QAQC Criteria. Includes the QC criteria checks for the continuous stream temperature data. These checks are summarized below.
6) Calculate Monthly SNN Statistics. Generates dataframe of summarized monthly statistics from the interpolated daily stream temperature measurements.
7) Save Output Files. Will save the site information and daily and monthly temperature data in the "results" folder.

The following QC criteria checks were applied in sequence with the aggregated stream temperature dataset:
Check 1) Expected temperature range: Remove all sub-daily measurements that are outside of an expected range of stream water temperature (< -1 and > 35 degrees C).
Check 2) Out-of-water measurements: Remove first and last day of measurement for each year, except when the first or last measurement of the year is at the beginning or end of the year.
Check 3) Sub-daily measurement completeness: Keep only days that have >= 90% hourly coverage after sub-hourly measurements were averaged. Interpolate between missing hours with maxgap of 2 hours
Check 4a) Deviations from expected sub-daily range: Remove days that have sub-daily (now hourly) measurements that range by more than 10 degrees C.
Check 4b) Deviations from expected change in range: Remove days that have a change in day-to-day range above 10 degrees C. 
Check 5) Erroneous measurements (out-of-water or burial): Remove hourly measurements based on > 3 sd residual of measured and predicted daily water temperature
Check 6) Daily completeness: Remove months lacking complete daily measurements for < 95% of the month. Calculate daily statistics and interpolate with maxgap of 2 days. 

Questions about code can be directed to:

Craig Connolly
connolly.craig@epa.gov

Naomi Detenbeck
detenbeck.naomi@epa.gov

## Credits
The Agency has unlimited rights to all custom-developed code produced. All comments expressed in this source code are the author's and do not necessarily reflect the policies and views of US EPA.

## Disclaimer
The United States Environmental Protection Agency (EPA) GitHub project code is provided on an "as is" basis and the user assumes responsibility for its use. EPA has relinquished control of the information and no longer has responsibility to protect the integrity, confidentiality, or availability of the information. Any reference to specific commercial products, processes, or services by service mark, trademark, manufacturer, or otherwise, does not constitute or imply their endorsement, recommendation or favoring by EPA. The EPA seal and logo shall not be used in any manner to imply endorsement of any commercial product or activity by EPA or the United States Government. 
