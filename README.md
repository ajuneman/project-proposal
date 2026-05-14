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

Audrey Juneman, Madeline Hulle, Maisie Prestridge
<<<<<<< HEAD

=======
>>>>>>> 4ae427792db78c366e2525dff542f43e9737161a

## Topic information and question

**Topic:**  

Hydrology/Water Quality

**Question(s):**  

How does precipitation explain salinity?

**Response variable(s)**

Monthly Mean Salinity (ppt)

## Datasets

**Datasets used:**

- NOAA-weather-data.csv
- NCOS_YSI_Water_Quality_Monitoring_).csv
- YSI_Data_Begin_1.csv

## Figures

**Potential figure 1:**

Our first figure has date (`date`) on the x-axis and total monthly precipitation (`sum_prcp`) on the y-axis, showing the variable precipitation at NCOS over time. This figure will contextualize when the rainy seasons occur and may help to explain salinity values further along.  

**Potential figure 2:**

Our second figure will have average monthly water surface elevation (`avg_elv`) on the x-axis and monthly average salinity (`mean_salinity`) on the y- axis. The purpose of this figure will be to see is the water surface elevation has an impact on the average salinity measured. This could inform the relationship between water volume and salinity. * Water surface elevation was only taken at the Venoco Bridge Site *

**Potential figure 3:**

Our third figure will display monthly average salinity (`mean_salinity`) on the x-axis and elevation code (`elevation_code`) on the y-axis. This figure will show how salinity levels differ at different depths within each of the sites.

**Potential figure 4**

Our fourth figure will be our ultimate answer to the question: How does precipitation explain salinity. We will have the monthly sum of precipitation (`sum_prcp`) on the x-axis and monthlt average salinity (averaged across elevation codes, `mean_salinity`) on the y-axis. We will facet this figure by site to visualize this relationship at all three locations and see if a relationship between the variables appears and how this may differ between sites. 

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


### Summarizing Precipitation

- create new object `monthly_prcp` from `water_quality_weather`
- only include elevation codes 0, 1, and 2
- get rid of site names w/ "other"
- removing any duplicate rows based on the combination of month, year, and wy
- grouping by month and year
- calculating the sum of prcp per month and year (not per site as it is a weather event and not site specific)
- ungroup data frame
- create a date column to be read as a date
- creating levels to `month`, putting them in a logical order


### Filtering Salinity Data Frame

- create a new object `salinity_filtered` from `water_quality_weather`
- filter to only include elevation codes of 0, 1, and 2
- exclude the salinity outlier global ids
- dropping the column for other_site_names
- get rid of site names w/ "other"
- relevel the factors in site_full to represent output better (lowest to highest avg salinity)
- take out sites w/ NA
- keeping only columns of interest (to reduce confusion)
- creating levels to `month`, putting them in a logical order
- group by site full, month, and year
- calculate mean salinity (per site per month)


### Joining Monthly Precipitation (sum) w/ Monthly Salinity (avg.)

- creating a new object `salinity_prcp` from `salinity_filtered`
- joining `salinity_filtered` with `monthly_prcp` by the columns month and year


### Make a Data Frame for Exploritory Purposes (Salinity)

- create new object `exploratory_salinity` from `water_quality_weather`
- only keep elevation codes 0, 1, 2, and 3 (keeping three just for visualization)
- filter the column `global_id` to exclude the values that match those in `salinity_outlier_ids`
- drop the column `other_site_names`
- filter out site names that match "other"
- relevel the factors in site_full to represent output better (lowest to highest avg salinity)
- filter out site names that match "N/A"
- keeping only columns of interest (to reduce confusion)
- group by site, elevation code, month, and year
- calculate mean salinity
- creating levels to `month`, putting them in a logical order

### Make a Data Frame for Exploratory Purposes (Water Surface Elevation)

- create a new object `monthly_surface_elv` from `water_quality_weather`
- only include elevation codes 0, 1, and 2
- filter out site names matchign to "other"
- group by site, month, and year
- calculate mean water surface elevation
- ungroup data frame
- create a date column to be read as a date
- creating levels to `month`, putting them in a logical order

### Joining `monthly_surface_elv` and `salinity_prcp`

- create a new object `elv_viz` from `monthly_surface_elv` 
- join to `salinity_prcp` by the columns month, year, and site full
- filter out all NA average elevation values

### Distribution (Comparing Groups) Visualization Plans

#### Boxplot w/ Jitter - Distribution of Mean Salinity Values Across Sites
- data: `salinity_prcp`
- x: `site_full`
- y: `mean_salinity`
- geom_boxplot() & geom_jitter()

#### Strip Chart w/ Standard Deviation - How Mean Salinity Differs Between Sites
- data: `salinity_prcp`
- x: `site_full`
- y: `mean_salinity`
- geom_jitter()
- stat_summary(fun = mean, geom = "point") & stat_summary(fun.data = mean_sdl, fun.args = list(mult = 1), geom = "errorbar")

#### Ridgeline Plot - Salinity Observation Distribution Across NCOS Sites
- data: `salinity_prcp`
- x: `mean_salinity`
- y: `site_full`
- geom_density_ridges()

### Distribution of Observations

#### # of Observations per Site

- data: `water_quality_weather`
- x: `site_full`
- y: `number_of_obs`
- geom_bar()

#### # of Observations per Elevation Code

- data: `water_quality_weather`
- x: `elevation_code`
- y: `number_of_obs`
- geom_bar()

### Figure Visualization


#### Figure 1. Total Monthly Precipitation at NCOS
- data: `salinity_prcp`
- x-axis: `date`
- y-axis: `sum_prcp`
- geometry: geom_point() & geom_line()

#### Figure 2. How Salinity Relates to Water Surface Elevation (Venoco Bridge)
- data: `elv_viz`
- x-axis: `avg_elv`
- y-axis: `mean_salinity`
- geometry: geom_point()
- facet_wrap(~`site_full`)

#### Figure 3. How Salinity Varies Between Elevation Codes
- data: `exploratory_salinity`
- x-axis: `elevation_code`
- y-axis: `mean_salinity`
- geometry: geom_point()
- stat_summary(fun = median, geom = "point") & stat_summary(fun = median, geom = "line")
- facet_wrap(~ `site_full`)

#### Figure 4. How Precipitation Explains Salinity
- data: `salinity_prcp`
- x-axis: `sum_prcp`
- y-axis: `mean_salinity`
- geometry: geom_point()
- facet_wrap(~ `site_full`)


## Project roles

**Natural history/framing director:**

Maisie

**Stats and visualization director**

Madeline

**GitHub/code director**

Audrey


## Elective (not required for all groups or group members)

**Group members completing elective:**

Madeline, Maisie, and Audrey

**Elective idea:**

Still in progress: a creative map project that depicts salinity throughout each pool, along with the rainfall and the species that are native to each pool. 

**Elective timeline (what you will have completed each week):**

Week 7: project proposal and workplan

Week 8 (timeline check in): a very detailed draft of what specifically we will be doing, times picked for when we are going to work on project, roles and materials brainstormed

Week 9: first "draft" of project, some work done individually or together

Week 10: presentation finished for project, project completed

Finals week: nothing (our project will be done)








