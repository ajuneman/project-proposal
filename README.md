# Group project proposal

**Spring 2026**

Directions:

- Use your work plan from class to fill in the information below.
- Practice pulling, making changes, staging/committing/pulling/pushing to the same repo.
- **Communicate about who is doing what throughout the entire process.**

What you will submit on Friday the 15th:

- proposal: a link to your forked repository with the completed proposal in the README
- work plan: your paper plan that you completed in class on Monday the 4th

Use your project proposal to:

- refer back to the original plan while you are working
- keep track of high-level changes in structure (e.g. role switching, elective modifications)

Note:

- your project proposal is subject to change after you learn more about your datasets and what is possible - allow yourselves the flexibility to make adjustments as needed
- the more detail you can provide in your proposal, the more thorough your feedback will be

## Group members

Audrey Juneman, Madeline Hulle


## Topic information and question

**Topic:**  

Hydrology/Water Quality

**Question(s):**  

How does precipitation explain salinity?

**Response variable(s)**

Salinity (ppt)

## Datasets

**Datasets used:**

- NOAA-weather-data.csv
- NCOS_YSI_Water_Quality_Monitoring_).csv
- YSI_Data_Begin_1.csv

## Figures

**Potential figure 1:**

Our first figure has date (`date`) on the x-axis and precipitation (`prcp`) on the y-axis, showing the precipitation at each site over time. This figure will contextualize when the rainy seasons occur. 

**Potential figure 2:**

Our second figure will have date (`date`) on the x-axis and water surface elevation (`water_surface_elevation`) on the y- axis. The purpose of this figure will be to compare the depth of each site and how they change over time. We plan to display this plot together with figure 1 to visualize the relationship between precipitation and water elevation. 

**Potential figure 3:**

Our third figure will display salinity (`salinity_ppt`) on the x-axis and elevation code (`elevation_code`) on the y-axis. This figure will show how salinity levels differ at different depths within each of the sites.

**Potential figure 4**

Our fourth figure will be our ultimate answer to the question: How does precipitation explain salinity. We will have precipitation (`prcp`) on the x-axis and salinity (averaged across elevation codes, `avg_sal`) on the y-axis. We will facet this figure by site to visualize this relationship at all three locations and see if the relationship is consistant throughout. 

## Data cleaning/wrangling/summarizing plan

### Cleaning 

#### Cleaning weather:

- create a new object from `weather`
- clean the column names
- make sure that date is understood as a date
- select the columns of interest
- extract month and year from date
- make new column for water year

#### Cleaning metadata:

- create new object from `metadata`
- clean column names
- exclude unneeded columns
- make sure monitoring date is understood as date
- rename monitoring date column to show that it has datetimes
- extract month and year from monitoring date
- create new column for water year
- create new column of full site name

#### Cleaning water parameters

- create new object from `water_parameters`
- clean column names
- exclude unneeded columns
- rename depth code to be elevation 

### Joining Data Frames 

- water quality = metadata + water parameters

then

- water quality weather = water quality + weather

### Taking care of Outliers

- create `salinity_outlier_ids` from `water_quality_weather`
- filter to only include outlier salinity values (> 1410)
- extract the global id column as a vector using pull()

### Filtering Salinity Data Frame

- create a new object `salinity_filtered` from `water_quality_weather`
- filter to only include elevation codes of 0, 1, 2, and 3
- exclude the salinity outlier global ids
- relevel the factors in site_full to represent output better

### Summarizing

#### Figure 1. 
- x-axis: `date`
- y-axis: `prcp`
- geometry: geom_point() & geom_line()

#### Figure 2. 
- x-axis: `date`
- y-axis: `water_surface_elevation`
- geometry: geom_point() & geom_line()

#### Figure 3. 
- x-axis: `salinity_ppt`
- y-axis: `elevation_code`
- geometry: geom_col()

#### Figure 4. 
- x-axis: `prcp`
- y-axis: `salinity_ppt`
- geometry: geom_point() & geom_line() +
- facets: facet_wrap(~ site_full)


## Project roles

**Natural history/framing director:**

Maisie

**Stats and visualization director**

Madeline

**GitHub/code director**

[delete this line and enter your own text here]


## Elective (not required for all groups or group members)

**Group members completing elective:**

Madeline, Maisie, and Audrey

**Elective idea:**

Still in progress: a creative map project that depicts salinity throughout each pool, along with the rainfall and the species that are native to each pool. 

**Elective timeline (what you will have completed each week):**

Week 7: project proposal and workplan

Week 8 (timeline check in): enter your own text here

Week 9: enter your own text here

Week 10: enter your own text here

Finals week: enter your own text here







