# Target data 

**Please Note: The target data description below pertains to the 2023-24 FluSight season. Target data for the 2024-25 season will remain focused on influenza laboratory-confirmed hospital admissions. However, updated details and links to current data sets will be provided as they become available.**

The target-data folder contains the laboratory confirmed influenza hospital admission ("gold standard") data that FluSight forecasts are eventually compared to. Influenza hospitalization data are taken from the [HealthData.gov NHSN Hospital Respitatory Dataset](https://www.cdc.gov/nhsn/psc/hospital-respiratory-reporting.html).

*Table of Contents*

-   [Hospitalization data](#hospitalization-data)
-   [Other data sources](#data-sources)
-   [Accessing target data](#accessing-target-data)


Hospitalization data
----------------------

### HealthData.gov Hospitalization Timeseries

FluSight hospital admission prediction targets (`wk inc flu hosp` and `wk flu hosp rate changes`) for this season will continue to be based on the influenza 'previous day's admissions with laboratory confirmed influenza virus infection' [Field 34](https://www.hhs.gov/sites/default/files/covid-19-faqs-hospitals-hospital-laboratory-acute-care-facility-data-reporting.pdf) reported through CDC's NHSN (the dataset formerly known as HHS-Protect), [HealthData.gov COVID-19 Reported Patient
Impact and Hospital Capacity by State
Timeseries](https://healthdata.gov/Hospital/COVID-19-Reported-Patient-Impact-and-Hospital-Capa/g62h-syeh).
These data are released weekly.



Previously collected influenza data from the 2020-21, 2022-23, and 2023-2024 influenza seasons (Fields 33-38) are included in the COVID-19 Reported Patient Impact and Hospital Capacity by State Timeseries dataset. Reporting of the influenza fields 33-35 became mandatory in February 2022, and additional details are provided in the current [hospital reporting guidance and FAQs](https://www.hhs.gov/sites/default/files/covid-19-faqs-hospitals-hospital-laboratory-acute-care-facility-data-reporting.pdf). Numbers of reporting hospitals increased after the period that reporting became mandatory in early 2022 but have since stabilized at high levels of compliance.  The number of hospitals reporting these data each day by state are available in the previous_day_admission_influenza_confirmed_coverage variable found in the COVID-19 Reported Patient Impact and Hospital Capacity by State Timeseries dataset.

Aggregated official counts are publicly released on Fridays. Preliminary counts are released on Wednesdays. Official counts can be revised in subsequent data updates.
These data are also available in a facility-level dataset; data values less than 4 are suppressed in the [facility-level dataset](https://healthdata.gov/Hospital/COVID-19-Reported-Patient-Impact-and-Hospital-Capa/anag-cw7u). 


*Please note the following detail from the [dataset description](https://healthdata.gov/Hospital/COVID-19-Reported-Patient-Impact-and-Hospital-Capa/g62h-syeh)*: 

"The file will be updated regularly and provides the latest values reported by each facility within the last four days for all time. This allows for a more comprehensive picture of the hospital utilization within a state by ensuring a hospital is represented, even if they miss a single day of reporting."  

This implies that some values may be repeated. Extra caution should be applied in these cases and in particular for interpreting data the most recent week of data, as hospitals report hospital admissions for the previous day (further detail below).


Some of these data are also available programmatically through the [EpiData](https://cmu-delphi.github.io/delphi-epidata/) API. See accessing target (truth) data section below for details.


Other data sources
------------

Influenza hospitalization admissions data is also available in a [facility-level dataset](https://healthdata.gov/Hospital/COVID-19-Reported-Patient-Impact-and-Hospital-Capa/anag-cw7u); data values less than 4 are suppressed in the [facility-level dataset](https://healthdata.gov/Hospital/COVID-19-Reported-Patient-Impact-and-Hospital-Capa/anag-cw7u). 

Percent of Emergency Department visits with a specified pathogen (COVID-19, Influenza, and Respiratory Syncytial Virus) out of all emergency department visits in a given epiweek are reported by the CDC National Syndromic Surveillance Program (NSSP) and provided in the [2023 Respiratory Virus Response - NSSP Emergency Department Visits dataset](https://data.cdc.gov/Public-Health-Surveillance/2023-Respiratory-Virus-Response-NSSP-Emergency-Dep/vutn-jzwm).  Weekly national-level Emergency Department Visits for these pathogens are available for download by age-group in this [landing page](https://www.cdc.gov/ncird/surveillance/respiratory-illnesses/index.html).   

Additional historical influenza surveillance data from other surveillance systems are available at [https://www.cdc.gov/flu/weekly/fluviewinteractive.htm](https://www.cdc.gov/flu/weekly/fluviewinteractive.htm). These data are updated every Friday at noon Eastern Time. The "cdcfluview" R package can be used to retrieve these data. Additional potential data sources are available in Carnegie Mellon University's [Epidata API](https://delphi.cmu.edu/).


### Data processing

The hospitalization target data is computed based on the `previous_day_admission_influenza_confirmed`
field which provides the new daily admissions with a confirmed diagnosis of influenza.

For each horizon of predictions, we will use the specification of
epidemiological weeks (EWs) [defined by the US
CDC](https://ndc.services.cdc.gov/wp-content/uploads/MMWR_Week_overview.pdf) which
run Sunday through Saturday. As an example, if 17 confirmed influenza hospital admissions were reported in the `previous_day_admission_influenza_confirmed` field in a row where the `date` field was 2023-11-15, then the target dataset would assign those 17 hospital admissions to a date of 2023-11-15. These hospital admissions would then be counted towards the weekly total computed for EW46 in 2023, which runs from 2023-11-12 through 2023-11-18. There are standard software packages to convert from dates to epidemic weeks and vice versa (e.g. [MMWRweek](https://cran.r-project.org/web/packages/MMWRweek/) for R and [pymmwr](https://pypi.org/project/pymmwr/) and [epiweeks](https://pypi.org/project/epiweeks/) for Python).


### Additional resources

Here are a few additional resources that describe these hospitalization
data:

-   [data dictionary for the
    dataset](https://healthdata.gov/Hospital/COVID-19-Reported-Patient-Impact-and-Hospital-Capa/g62h-syeh)
-   the [official document describing the **guidance for hospital
    reporting**](https://www.hhs.gov/sites/default/files/covid-19-faqs-hospitals-hospital-laboratory-acute-care-facility-data-reporting.pdf)


Accessing target (truth) data
----------
While we make efforts to create clean versions of the weekly target data, these should be seen as secondary sources to the original data at the HHS Protect site. National hospitalization, i.e., US, data are constructed from these data by summing the data across all 50 states, Washington DC (DC), and Puerto Rico (PR). Note that due to low counts in additional territories, such as the US Virgin Islands (VI) and American Samoa (AS) will not be included.       
Note that reported data are occasionally revised as data are updated. Please see appendix below for information on accessing archived data versions.


### CSV files
A set of comma-separated plain text files are automatically updated every week with the latest observed values for incident hospitalizations. A corresponding CSV file is created in `target-data/target-Incident-Hospitalizations.csv`.


### Resources for Accessing Hospitalization Data

Version history of NHSN hospitalization data can be accessed through the [COVID-19 Reported Patient Impact ad Hospital Capacity by State Timeseries Archive Repository](https://healthdata.gov/dataset/COVID-19-Reported-Patient-Impact-and-Hospital-Capa/qqte-vkut/about_data).Our collaborators at the [Delphi Group at
CMU](https://delphi.cmu.edu/) have provided resources to make these data (as well as archived versions) available through their [Delphi Epidata
API](https://cmu-delphi.github.io/delphi-epidata/). State-level hospitalization time series data prepared from the old reporting form, as well as prior versions of this time series, are available under the ["covidcast" endpoint of the API](https://cmu-delphi.github.io/delphi-epidata/api/covidcast.html) with the ["hhs" data source name](https://cmu-delphi.github.io/delphi-epidata/api/covidcast-signals/hhs.html) and `confirmed_admissions_influenza_1d` signal name.

**Note:** The covidcast "hhs" data source shifts dates so that the time_values reflect date of admissions, not the date after admissions for the time period when data was reported using the previous_day_admission_influenza_confirmed field.

Various other data sets and their version histories, including data sets not available elsewhere, are available and continue to be added to the Epidata API.  For influenza this season, these include:

- [National Syndromic Surveillance Program data](https://cmu-delphi.github.io/delphi-epidata/api/covidcast-signals/nssp.html)
- [Google Symptoms data](https://cmu-delphi.github.io/delphi-epidata/api/covidcast-signals/google-symptoms.html)
- [Additional fields for state-level HHS&NHSN hospitalization reporting (old form)](https://cmu-delphi.github.io/delphi-epidata/api/covid_hosp.html) 
- [Facility-level hospitalization reporting (old form)](https://cmu-delphi.github.io/delphi-epidata/api/covid_hosp_facility.html) 
- [Facility lookup](https://cmu-delphi.github.io/delphi-epidata/api/covid_hosp_facility_lookup.html)

Other data available in the API are listed in the [API documentation](https://cmu-delphi.github.io/delphi-epidata/) and [signal discovery tool](https://delphi.cmu.edu/signals/) (in development). To access these data, teams can utilize the [epidatr R package](https://cmu-delphi.github.io/epidatr/) or [epidatpy Python package](https://cmu-delphi.github.io/epidatpy/). Some basic examples of pulling old influenza data are available [here](https://github.com/cmu-delphi/flusight-helper-snippets).

