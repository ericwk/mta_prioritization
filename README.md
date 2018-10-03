# mta_prioritization
Identify best New York MTA stations for WomenTechWomenYes to recruit attendees for their annual gala.

### Collaborators
Eleanor Hayes, Aditya Chaudhary

### Background
We have been asked by WomenTechWomenYes (WTWY), a nonprofit organization promoting involvement of women in technology, to propose how to identify which MTA subway stations in New York City to give priority to for assigning street teams to gather email addresses of potential attendees for their annual gala.  WomenTechWomenYes wants to fill their event space with individuals passionate about increasing the participation of women in technology, and concurrently build awareness and reach of their organization. They want the publicly available turnstile data from MTA to be used plus any other data sources that may support their goal.

### Analysis Approach
We will determine which MTA stations have the most entry volume from the turnstile data available from the MTA website.  We will combine data for foursquare checkins (available from foursquare API) from  New York City businesses of interest to women with station entry volume to prioritize stations for focus by WTWY.

### MTA Data File Description
* Provides counter outputs for consecutive dates and times during a week for entries ('ENTRIES') and exits ('EXITS'), and the date ('DATE') and time ('TIME') for when the counts are provided, from MTA subway station turnstiles.
* Provides station name ('STATION') and three identifier codes ('C/A', 'UNIT', 'SCP') that can be combined to identify a unique turnstile.
* Provides data that was not used in the analysis ('LINENAME', 'DIVISION', 'DESC')

### foursquare Data File Description
* Provides checkin timestamp with venue location longitude and latitude.
* Provides category for each venue.

### Notebooks
The analysis is carried out via a series of six Jupyter notebooks:

**00-import_and_pkl_raw_data.ipynb**
Reads turnstile counter data from New York City MTA web site for weeks ending on the Saturdays of April 28, 2018 through June 30, 2018 (eight weeks) to be used in the analysis.  This was the most recent data available and represents the same period in which WTWY will collect emails next year.  Stores data in a pickle file.

**01-clean_and_get_weekly_totals**
* Filters MTA station entry data to select data for high traffic times of Noon to 9 PM when tech workers are most likely to be using the subway for lunch or after work errands, or for commuting home.
* Groups data into count series by unique turnstile (identified by combination of 'STATION', 'C/A', 'UNIT', and 'SCP' identifiers in the data files) and by day (Noon to 9 PM).
* Calculates daily entry volume for each turnstile by subtracting the minimum count from the maximum count for each day.
* Replaces daily entry data for 28 turnstiles (< 1% of total number of turnstiles) that showed anomalous (millions versus a population mean of thousands) with average daily entries for these turnstile for other days.
* Calculates daily entry volume for each station by summing daily turnstile entry volume for all turnstiles in a station and groups sums by week.
* Drops weekly data for stations that do not have seven days of data present.
* Calculates weekly average entries for each station over analysis period.
* Stores dataframe with weekly average entries by station in pickle file.
* Identifies top ten stations by average weekly entry volume that represent 22% of total weekly subway volume over this period.

**02-import_foursquare_data.ipynb**
* Reads comma delimited text file taken from foursquare API that provides 2014 check in data (most recent available).
