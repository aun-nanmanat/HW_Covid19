# Comprehensive Analysis of COVID-19 Pandemic Dynamics and Socio-Economic Impacts Using WHO and WDI Data
## Project Overview
### Summary
Analyzed the evolution of COVID-19 cases and deaths across several countries using WHO data and examined the socio-economic impacts using World Bank Development Indicators. Implemented data smoothing, correlation analysis, and PCA to uncover trends and insights, focusing on the differential impacts on various countries and the relationship between health metrics and socio-economic indicators."

### Key Skills and Techniques
- Data Analysis

- Data Visualization

- Statistical Analysis

- Correlation Analysis

- Principal Component Analysis (PCA)

- Time Series Analysis
  
### Methodology
- **Data Collection:** Aggregated daily COVID-19 cases and deaths from WHO and socio-economic indicators from WDI.

- **Data Cleaning:** Handled missing values and ensured data consistency for reliable analysis.

- **Data Smoothing:** Applied smoothing techniques to highlight trends and reduce noise in COVID-19 case data.

- **Correlation Analysis:** Investigated correlations between COVID-19 metrics and socio-economic indicators.

- **Principal Component Analysis (PCA):** Conducted PCA to reduce dimensionality and identify key patterns in the socio-economic data.

- **Visualization:** Created informative graphs and plots to illustrate key findings.
  
### Key Findings and Insights
- Identified the peak and decline periods of COVID-19 waves in various countries.

- Demonstrated how different smoothing windows affect the interpretation of COVID-19 trends.

- Revealed the evolving relationship between COVID-19 cases and deaths over time.

- Highlighted significant socio-economic correlations, such as the relationship between physician density and life expectancy.

- Showed how PCA can distinguish between countries based on socio-economic characteristics.

### Visuals and Figures
- Time series plots of COVID-19 cases and deaths with different smoothing windows.

- Correlation matrices illustrating relationships between socio-economic indicators.

- PCA plots showing country clusters based on socio-economic metrics.

### Impact
- Provided insights into the effectiveness of public health interventions across different countries.

- Highlighted the importance of healthcare infrastructure in mitigating pandemic impacts.

- Informed policy decisions by demonstrating socio-economic factors influencing health outcomes.
  
# Data

Two data sets are provided in the repository:

1. Daily numbers of new cases of COVID-19 and new COVID-19 related deaths in most countries in the world as collected by the World Health Organization (WHO) on a daily basis. The data is from 2022-09-30. 
2. World Bank Development Indicators (WDI) with some socio-economic indicators as provided for most countries in the world. 


# Exercises

1. **Structure a report:** This exercise is about creating a skeleton of the document which you fill out following the next exercises. Create a quarto document taking the YAML header following this [advice](https://cu-f23-mdssb-01-concepts-tools.github.io/Website/reports.html). Create a chunk with the label `data` directly below the YAML. In the following you will use this chunk for 
    - loading packages
    - wrting your functions
    - loading the two data frames `who` and `wdi` and doing all data manipulations and additions which are not only for one visualization
    
    Now, create two first level (`#`) sections below: "COVID-19 time evolution" and "World Development Indicators".
    
2. Five exercises on the time evolution of COVID-19: Write one section with a descriptive headline (`##`) for each exercise.
    1. **Covid Cases and Deaths in Germany's 1st wave**: Plot a time series of the number of daily cases and daily death in Germany before 2020-08-31 in one diagram. (Hints for R: For plotting cases and deaths in one diagam use `pivot_longer` before the plot.) Describe the development of cases in Germany answering questions like: When did the wave start? When did it end? When was the peak? How many cases were there at the peak? How many deaths were there at the peak? What are the fluctuations and why could they be there?
    2. **Cumulative cases in Germany, Italy, France, and UK**: Plot the cumulative cases for the four countries until 2020-08-31s in one panel. Provide the cumulative deaths in another panel. Describe how the pandemic in Italy, France, and UK unfolded differently than in Germany.  (Hints for R: In the chunk `data`, create additional variables for the cumulative cases and cumulative deaths. Do not forget to group by country! Use `pivot_longer` for cumulative cases and deaths before the plot and then `facet_wrap` by cases and deaths.)
    3. **Smoothing daily data**: Create a three functions `smooth3`, `smooth7`, and `smooth7` which smooth the data with *moving averages* of the last 3, 7, and 10 days. The function should take a timeseries as a vector `function(x)`. Then use these function to create new variables `New_cases_smooth3`,  `New_cases_smooth7`,  `New_cases_smooth10`,  `New_deaths_smooth3`,  `New_deaths_smooth7`,  and `New_deaths_smooth10`. Do all of this in the chunk `data` and do not forget to group by country! (Hints for R: To compute a moving average for a time series `x` for the last 3 days, use `1/3 * (x + lag(x, n = 1, default = 0) + lag(x, n = 2, default = 0))`.)   
    In the section "COVID-19 time evolution", make a plot to compare the smoothed three types of smoothed daily cases for Germany until 2020-08-31. Make one panel for each smoothing window (3 days, 7 days, 10 days). Describe: Which window smoothes the data best? Normally, the more days you average, the smoother will be the data. Why is this not the case here?
    4. **How do deaths follow cases?**: Create a new variable `shiftscale_cases` which is a shifted and scaled version of `New_cases_smooth7`. Adjust the shift and the scale parameter such that the plots for `shiftscale_cases` and `New_cases_deaths_smooth7` overlap as good as possible for the days when cases where increasing exponentially (roughly second half of March). How to in R: Write a function `shiftscale <- function(x, shift, scale)` where you shift the timeseries `x` using `lag` by `shift` days and scaling the magnitude of `x` by multiplication with `scale`. Then create the new variable using `shift = 0` and `scale = 1` and plot the time series of `shiftscale_cases` and `New_cases_deaths_smooth7` in one panel. This plot should look like the smooth version of the plot from the first visualization. Now play with different numbers for `shift` and `scale` until you reach the best overlap of both graphs focussing on the time in the second half of March. Only put your best solution in the report. Describe: Write down the parameters you used to shift and scale cases. How well is the overlap in the exponential growth phase? How can you use your two numbers for `shift` and `scale` to describe the relation between deaths and cases?    
    Now, create a second visualization where you use the same shift and scale but plot the time from 2020-07-01 to 2020-12-31 in Germany. Do the two time series still overlap? If not, how do they differ? What has changed in the relation between cases and deaths in the second wave?    
    (BONUS) Select 2 countries of your choice and repeat the exercise to *fit* the shift and scale parameters by guessing and visual assessements in the first wave's exponential growth phase. (Hint: First, assess visually a good time range to show for thes particular countries.) Are the parameters similar to the ones you found for Germany? 
    5. **When does the wave break?** Look at the change of the smoothed (7 days) new cases (= derivative of new cases = 2nd derivative of cumulative cases) in Germany in the time range 2020-03-01 to 2020-04-15. Compute the variable `Diff_cases_smooth7` (Hint for R: Subtract `lag(x, n = 1)` from the time series `x` to compute the derivative.) and make a visualizion with two panels, one for the smoothed new cases and on for the diff. Describe how the two two graphs are related. At what day did was the peak of the diff, at what day was the peak of the smoothed new cases? (Visualization hint: Use `+ scale_x_date(date_breaks = "2 days", date_labels = "%d")` to show detailed days on the x-axis. 

3. Exercises on the World Development Indicators: Write one section with a descriptive headline (`##`) for each exercise. The exercises are based on the data set `wdi` which you created in the chunk `data`, however the variable names are very long. For convenience you can `rename` them to shorter names, for example `urban_pop, rural_pop, pop_lower_half_median, pop, pop_older65, pop_density, physicians_per_1000, life_expectancy, gdp_per_capita`. 
    1. Make a plot of the correlation matrix of all numerical indicators in the WDI data set. (Hint for R: Use `cor` to compute the correlation matrix and `corrplot` to plot it.) Describe: Which indicators are correlated? Which are not? What does this mean? (Use `correlation` from the package `correlation` and plot the results using `|> summary(redundant = TRUE) |> plot()`.) Describe some interesting correlations. There are two variables which are essentially one, can you spot them in the correlation matrix?
    2. For PCA's we need to remove NA's. We can either remove countries with NA's or variables with NA's. The first option is not very good, because we would lose many countries. The second option is not very good, because we would lose many variables. So we will do both and compare the results. Create a data frame `wdi_noNA_small` where you remove all countries with NA's (use `na.omit`), and a data frame `wdi_noNA_large` where you first remove the two variables with most NA's (find out using `summary(wdi)`) and then the countries with remaining NA's. 
        1. Write a sentence which explains how many countries are in each data frame. (Hint: Use [inline code](https://quarto.org/docs/computations/execution-options.html#inline-code) like `` ` `` `r nrow(wdi_noNA_large)` `` ` ``). Make a table for the small data frame where you list all countries which are in `wdi` but not in `wdi_noNA_small` and which have more than one million population. (Hint: Use `anti_join` to select the rows of `wdi`, use `knitr::kable` to make the table nice in the output.). Are large countries missing? Make the same type of table for the data frame `wdi_noNA_large`. 
        2. Make a subsubsection (`###`) "PCA small" where you visualize the explained variance, the first four principal components from the rotation matrix, and the countries in the coordinates of PC1 and PC2. For each describe your main observations. In particular describe, what PC1 and PC2 represent. (Visualization hint: Color by `region` and use the additional aesthetic `label = iso3c`. Then use `geom_text(size = 3)` instead of `geom_point`. That you can see the country's iso codes to describe where some countries lie.)
        3. Repeat the former exercise for the data frame `wdi_noNA_large`. 
