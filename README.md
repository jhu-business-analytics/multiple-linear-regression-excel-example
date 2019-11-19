# Multiple Linear Regression with Excel

## Overview

In a simple linear regression we create a model that helps us predict a dependent variable from one independent variable. However, in most cases, our dependent outcomes will be fairly complex so our outcomes will depend on more than only one independent variable. We can use a __multiple linear regression__ analysis to incorporate several independent variables in our regression model to better forecast our dependent variable outcomes. 

Our simple linear regression models use two coefficients--the intercept and the slope--to adjust our independent variables and predict the values of our dependent variables. 

```
dependent_variable = intercept + slope*independent_variable
```

A multiple linear regression follows the same logic, however, we incorporate a new coefficient for each independent variable that we aim to incorporate into our model. 

```
dependent_variable = intercept + coefficient_1*independent_variable_1 + coefficient_2*independent_variable_2 + coefficient_3*independent_variable_3 + … + coefficient_n*independent_variable_n
```

Where n is the number of independent variables that we’ve included in our model.

We’ll build off of our simple linear regression model where we used Fiscal Year 2018 Baltimore City employee salary data. If you’re unfamiliar with the data and the example, you can review it [here](https://github.com/jhu-business-analytics/simple-linear-regression-excel-example). 

The data sets used in this example are available as raw data in csv or excel files [here](https://github.com/jhu-business-analytics/multiple-linear-regression-excel-example/tree/master/raw_data) (except for the 911 data, which is too large to upload to a GitHub repo, but available [here](https://data.baltimorecity.gov/Public-Safety/911-Police-Calls-for-Service/xviu-ezkt)). The [aggregated salary data](https://github.com/jhu-business-analytics/multiple-linear-regression-excel-example/tree/master/bmore_salaries_2011_2018) is created from aggregating all of the Baltimore City government [salary data](https://data.baltimorecity.gov/browse?category=City+Government) from fiscal year 2011-2018 in [this](https://github.com/jhu-business-analytics/multiple-linear-regression-excel-example/blob/master/2019-11-13-melanieshimano-baltimore-salary-2011-2018-combined.ipynb) python notebook, exported as a csv [here](https://github.com/jhu-business-analytics/multiple-linear-regression-excel-example/blob/master/total_bmore_city_salaries_2011_2019.csv). 

These differ slightly from our in-class examples which are available [here](https://github.com/jhu-business-analytics/multiple-linear-regression-excel-example/blob/master/baltimore_city_salaries_fy2011_fy2018_updated.xlsx) (initial data set) and [here](https://github.com/jhu-business-analytics/multiple-linear-regression-excel-example/blob/master/baltimore_city_salaries_fy2011_fy2018_with_regression.xlsx) (solution data set).

## Multiple Linear Regression 

At the end of our simple linear regression example with Excel, we created two simple linear regression models: 

1. We created a model to predict a Police Officer’s actual (__gross__), or contracted (__annual_rt__), salary in the Baltimore Police Department during Fiscal Year 2018 based on the number of years that the employee had worked in city government. This gave us a model that can be used to predict approximately 70% of our data (R2 = 0.7).

2. We created a similar model, but we tried to predict a Police Officer’s gross, or total earned, salary in the Baltimore Police Department during Fiscal Year 2018 based on the number of years that the employee had worked in city government. The gross salary might differ from the actual salary due to overtime earnings, unearned salary, or another condition in which the employee would earn either more or less than their contracted salary. This gave us a model that can be used to predict approximately 39% of our data (R2 = 0.39).

Neither of these models can predict 100% of either the employee’s actual or gross salary from the number of years that the employee has worked for Baltimore City government, so there must be additional variables that we can add to our model to better predict these dependent variables (e.g. annual and gross salary). Because our gross salary prediction is much more variable than our actual salary prediction, we *aim to create a multiple linear regression model to forecast a Baltimore Police Department Police Officer’s gross salary in any given fiscal year*. 

## Identifying Additional Independent Variables and Data Cleaning

We know that several variables such as the police officer’s station or route, age, number of shifts worked, or performance evaluations might highly predict their salary, however, this data is not easily accessible and is not available on Baltimore Open Data. After some preliminary research we found that factors such as the minimum wage, the city’s population or ratio of Baltimore Police Officers to the population, the number of arrests, the employee’s contracted salary, in addition to the number of years that the employee has worked for Baltimore City government might provide a better model to predict the employee’s gross earned salary. 

Since some of these factors are fiscal year-dependent, we combine all of the Baltimore City salary data (from Fiscal Year 2011-2019) in a single data set to expand our data and see how significant each of these values are in predicting the gross salary over 9 years. 

We retrieve additional raw datasets from the following sources and merge all of the information using pivot tables for individual data sets and VLOOKUP functions to add these new variables to the aggregated Baltimore City Salary dataset: 
 - Baltimore City population: [American Community Survey](https://factfinder.census.gov/faces/nav/jsf/pages/index.xhtml)
Minimum wage: [Federal Reserve Economic Research](https://fred.stlouisfed.org/series/STTMINWGMD) 
 - Number of arrests in Baltimore City: [Baltimore City Open Data - Arrest Data](https://data.baltimorecity.gov/Public-Safety/BPD-Arrests/3i3v-ibrt)
 - Number of 911 Calls in Baltimore City: [Baltimore City Open Data - 911 Calls](https://data.baltimorecity.gov/Public-Safety/911-Police-Calls-for-Service/xviu-ezkt)
 - Fiscal year: [Baltimore City Open Data - Employee Salaries](https://data.baltimorecity.gov/browse?category=City+Government)

This final, merged dataset is available in this repository [here](https://github.com/jhu-business-analytics/multiple-linear-regression-excel-example/blob/master/total_bmore_city_salaries_2011_2019.xlsx) under the “bmore_police_officer_lookup” sheet.

## Multiple Linear Regression Analysis in Excel
Now that we have all of our data, we’ll use the Excel Data Analysis ToolPak Add-on to calculate the coefficients for our multiple linear regression model and their significance, among other factors with our expanded data set.

### Data
Our aggregated data set includes the following columns:
 - __employee name__: name of the Baltimore City employee
 - __jobtitle__: Baltimore City job classification 
 - __deptid__: Employee’s department ID
 - __dept_name__: Baltimore City department name
 - __hire_dt__: Date employee was originally hired with Baltimore City government (not necessarily that department)
 - __annual_rt__: employee’s annual salary as indicated in their work contract
 - __gross__: employee’s actual earned income (may be more or less than the annual_rt salary)
 - __fiscal_year__: fiscal year for the indicated earnings and salary
 - __years_in_gov__: number of years the employee has been employed with Baltimore City government
 - __count_of_911__: number of 911 calls during that fiscal year
 - __%emergency_911__: fraction of classified emergency 911 calls during that fiscal year
 - __%high_911__: fraction of classified high 911 calls during that fiscal year
 - __%med_911__: fraction of classified medium 911 calls during that fiscal year
 - __%low_911__: fraction of classified low 911 calls during that fiscal year
 - __%non_emergency_911__: fraction of classified non-emergency 911 calls during that fiscal year
 - __count_arrests__: number of arrests in Baltimore City during that fiscal year
 - __%female_arrests__: fraction of female arrests in Baltimore City during that fiscal year
 - __%male_arrests__: fraction of male arrests in Baltimore City during that fiscal year
 - __min_wage__: minimum wage of Baltimore City during that fiscal year
 - __baltimore_population__: population of Baltimore city as of July 1, fiscal year year
 - __count_of_police__: number of police officers in BPD during that fiscal year, where “Police Officer” is defined as the *jobtitles* “Police Officer,” “POLICE OFFICER EID,” “Police Officer EID,” and “Police Officer Trainee” (filtered with an Excel Filter and copied into the regression dataset)

and looks like this: 

[! alt text]() photo of entire dataset

All of this data for all Baltimore City employees is in the __“all_bmore_fy”__ sheet tab; the other sheet tabs in this Excel sheet include copied pivot tables from the external sheets outlined here: 

- __regression_output__: initial multiple regression output from this tutorial
- __variable_correlation__: correlation between all of our independent variables
- __police_officer_reg_dataset__: initial dataset used for the regression analysis (this includes only fiscal years 2014-2019, and adds the *gross* column before the *annual_rt* column to format for the Data Analysis ToolPak)
- __revised_regression_output__: final multiple regression output from this tutorial
- __revised_reg_dataset__: revised data set to perform the final multiple regression analysis (this removes columns that are too highly correlated in the *variable_correlation* tab
- __all_bmore_fy__: the full salary data set export from the python notebook and Baltimore City salaries from fiscal years 2011-2019
- __bmore_police_officer_lookup__: salary data + corresponding external data from other tabs/data sets via VLOOKUP for only BPD Police Officer data
- __police_count_pivot__: pivot table to count the number of police officers in each fiscal year based on *police_officers_only* sheet
[! alt text]() *screenshot of police officer pivot
- __police_officers_only__: filtered data to include only Police Officer positions from the BPD
- __copy_of_pivot__: copy of *police_count_pivot* for format numbers for the fiscal year
- __911_pivot__: copy of the pivot table from 911 data (screenshot below) and correlation between variables
[! alt text]() *screenshot of 911 pivot
- __arrest_pivot__: copy of the pivot table from the arrest data (screenshot below) and correlation between variables
[! alt text]() *screenshot of arrest pivot
- __min_wage_fred__: minimum wage in Baltimore City, MD during for each fiscal year
- __baltimore_population__: population in Baltimore City during each fiscal year; this data is transposed and cleaned as shown below:

The original Baltimore City population data downloaded from the American Community survey looks like this: 

[! alt text]() * screenshot of baltimore population spreadsheet

This contains all of the information we need, however, this lists the fiscal years as column headers instead of as items in a “year column.” We can reformat the information to include the fiscal years in one column and the population in the next column, with an Excel `=TRANSPOSE()` function. This *transposes* the data to include the rows as columns (or columns as rows). 

To do this in the PEP_population_estimate_acs_in_class.xlsx workbook, we first highlight the range of cells where we want to transpose our data. We want to transpose a 9 column x 2 row data set into a 2 column x 9 row dataset, so we:
1. Type new column headers for our transposed data (*fiscal_year* and *population*)
2. Highlight a range of 9 rows in 2 columns (here A8:B16) where we want our transposed data to end up
3. *Without clicking outside of our highlighted cells,* type `=TRANSPOSE(F2:N3)` by typing `=TRANSPOSE()` and highlighting the cells (F2:N3) that we want to transpose
4. __Typing CTRL+SHIFT+ENTER__ *instead* of only ENTER to fill in the data; you’ll notice that this adds {} on the outside of your formula, because this is an *array* formula (e.g. the formula is applied this highlighted group of cells). Note: if you only type ENTER, you’ll get a #NUM error in the first cell. 

These are shown in the gif below:

[! alt text]() * gif of transpose

We then use the *text to columns* button under the Data tab to quickly edit the first column to include only the fiscal year, and to convert the fiscal year data to numbers as shown below.

[! alt text]() ** gif of editing the fy column

Additionally, because all of our external data (911 calls, arrests, minimum wage, population) are not available for all fiscal years, we restrict our data to fiscal years 2014-2019.

## Multiple Linear Regression with Excel Data Analysis ToolPak
Now that we have our cleaned dataset (tab __police_officer_reg_dataset__), we can use the __Data Analysis TookPak__ to quickly perform a multiple linear regression analysis with our data. 

#### Installing the Excel Data Analysis ToolPak

If you don’t already have the Data Analysis ToolPak installed in your version of Excel, you can add this in by clicking on the Tools Menu > Excel Add-in, checking the box for Analysis ToolPak, and then clicking OK as shown below. 

[!alt text]() Gif of installing toolpak 

If this is not an option on your version of Excel, you can install the Data Analysis ToolPak by:

1. Click File > Options
2. Click Excel Add-Ins in the Manage box, and choose Go
3. Select Analysis ToolPak in the Add-Ins dialog box, then click OK.

Once you have this installed, access the Data Analysis ToolPak by clicking on the Data menu and then the Data Analysis icon as shown in the gif above. 

## Multiple Linear Regression with Baltimore Police Department Data

1. In the __police_officer_reg_dataset__ tab, click on the Data Analysis button under the Data menu
2. Select Regression from the menu and click OK. Now, you’ll fill in the data that you want to include in your regression and identify any other important factors shown in the window below.
3. Fill in the following data to build the regression model
    - The __Input Y Range__ is our dependent variable that you’d like to predict. Here we are trying to better predict the gross (actual earned) salary, so the values in column F are filled in here; make sure to include the first row (column header) in this selection.
    - The __Input X Range__ are our independent variables that we are using as inputs for our model. Here we’re using all of the variables in columns G-U to build our model; make sure to include the first row (column header) in this selection.
    - Check the Labels box to indicate that we’ve selected the column header in our selections, which also labels our coefficient values in our regression output.
    - Select to send the regression output in a new worksheet labeled *regression_output*
    - Check the Residuals box to also get the residuals (or errors) in our regression output
    - Click OK. The regression output will populate a new spreadsheet

The above steps are demonstrated in the gif below:

[! alt text]() * gif of regression toolpak

Note that the Data Analysis ToolPak is a powerful tool, however, we need to make sure that our data is formatted correctly for it to work. This means we need to have all of the independent variables in one “block” of cells. The data in the [bpd_policeofficer_regression_template.xlsx](https://github.com/jhu-business-analytics/multiple-linear-regression-excel-example/blob/master/bpd_policeofficer_regression_template.xlsx) Excel workbook is set up to accommodate. 

## Interpreting Multiple Linear Regression Analysis Output
Our output from the Data Analysis ToolPak is:

[! alt text]() * screenshot of multiple linear reg

This gives us a lot of information, but the red, outlined values are the main values that we’re concerned with here:

 - The __R squared__ value is the same as described in our simple linear regression analysis. This tells us how well our regression model fits our data. Here it’s about 0.435, which means that this model describes approximately 43.5% of our data.
- The __Significance F__ tells us how likely that our coefficients are not useful in predicting our Y (independent variables). Our Significance F of 0 tells us there is “0” chance that our coefficients are not useful to predict Y, which means that they are significant.
-  The __Coefficients__ are the values that give us the best fit line estimate of our multiple linear regression model. These values mean that by only looking at the coefficients, a potential line equation to fit our data is: 

```
gross = -21902538 + 10844.2481(fiscal_year) + 2196.91785(years_in_gov) - 0.0860151(count_of_911_calls) + 0(%emergency_911) + 0(%high_911) + 0(%med_911) + 0(%low_911) + 0(%non_emerg_911) -0.2131958(count_arrests) + 0(%female_arrests) + 0(%male_arrests)  + 0(min_wage) +0.08218373(baltimore_pop) + 23.800253(count_of_police)
```
 - This is assuming that all of our selected variables are significant. The __P-values__ tell us the significance of each independent variable in building the regression model to predict the gross salary. A P-value less than 0.05 usually indicates that a variable is significant. If a variable is not significant, we don’t include that coefficient in our model equation.

In our data, we notice that some of the P-values are #NUM! errors. This is most likely due to our independent variables being too closely correlated with each other. Independent variables that are too closely correlated make it difficult to determine which of the closely correlated variables is contributing most to the best fit line. If some of our variables are too closely correlated, then we should only use one of these variables in our linear regression model. 

### Correlation

Although we can see that the 911, Arrest, and Population data are closely correlated because of the #NUM! Errors, we might want to look at how all of the independent variables are correlated to determine which variables to keep in our model. To do this, we use the __Data Analysis ToolPak__ again.

1. Click on the Data Analysis button under the Data Tab and choose *Correlation*
2. Select the cells we want to calculate the correlation for (here we’re looking at the 911 data), including the column headers
3. Check the Labels box
4. Indicate where you want this output to be: here we keep the output in the same workbook in cell A25, but you can also put this output in a new sheet similar to our regression analysis)
5. Click OK

See these steps in the gif below: 

[! alt text]() * gif of correlation

### Revising the Dataset
After looking at all of the correlating variables, we decide to only keep the following variable columns and re-run our Data Analysis ToolPak analysis: 
 - intercept
 - annual_rt
 - fiscal_year
- years_in_gov
- count_of_911_calls
- count_arrests
- min_wage
- count_of_police 

Our revised output is below: 
[! alt text]() **screenshot of new regression output

We see that our new __R squared__ value is __0.545__, which means that our revised model describes approximately 54.5% of our data. Additionally, we note that there no #NUM! Errors, however, __fiscal_year, years_in_gov, and count_of_911_calls__ all have p-values higher than 0.05, so we exclude these from our regression model to get:

```
gross = 10232927 + 1.85897305(annual_rt) - 0.3815128(count_arrests) + 10645.5257(min_wage) + 26.1251449(count_of_police)
```
The __Residual Output__ section gives us the *predicted gross* value from our regression equation and the residual (the predicted/calculated value subtracted from the observed/actual value), also known as the error. From these values we can determine which observations are considered outliers to further bolster our analysis. 

## Next Steps
Now, based on our model, we suggest that gross salary:
- Increases by $1.86 for every $1 that the annual salary increases
- Decreases by $0.38 for each arrest in Baltimore City
- Increases by $10,645.53 for each $1 that the minimum wage increases
- Increases by $26.13 for every police officer added to the total police officer population

The relationship between the salary and the minimum wage or the gross salary and the annual salary intuitively makes sense because increasing salaries should suggest increasing salaries. However, relationships such as the decrease in salary with increasing arrests or increasing salary for increasing police force don’t follow an expected pattern. We may expect police officer salaries to increase if there are more arrests because this might mean that they are potentially spending more overtime hours on increasing arrests. We might also expect that police officer gross salaries decrease when there are more police officers since there are more police officers to cover shifts, which would mean less overtime, or general, hours worked. 

To further understand these trends, we may want to look into the distribution of arrests around Baltimore and how the spread of arrests around the city has or has not shifted over the fiscal years. For example, if arrests have shifted from concentrated pockets in the city to evenly distributed around the city, then this may explain a slight increase in gross salary for lower arrests since police officers have to cover larger areas. Additionally, we may want to add other variables to our linear regression to see if factors such as population density, police officer:population ratio, average police sergeant salary, demographic trends, or arrests in specific city quadrants have significant effects on gross salary.


