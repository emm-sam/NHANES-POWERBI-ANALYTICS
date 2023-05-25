# NHANES-POWERBI-ANALYTICS

## Project Overview

- this project is about getting to grips with the most commonly used graphs and statistical techniques
- not neccessarily what researchers would be most interested in
- start using DAX, methods, relationships, visual features
- interpretation of results

### About NHANES II
- put a link to the dataset
- inclusion, exclusion criteria
- remember to eplain the data columns

### Caveats
- widely used dataset, available on kaggle
- already clean 
- alalytics projects already exist for this dataset but I did not look at these beforehand
- I did use educational material provided by others on youtube, medium, blogs
- no access to additional graphs 

### Learnings and focus for future 
- how to custom order a legend
- using parameters instead of filters
- time based graphs and trends
- running speed / efficiency 

### credits to others
- list your resources 

## Set-up 
- downloaded datasets from kaggle, csv format
- link
> Home -> Get Data -> text/csv -> connect -> open -> load
- only connected 2 out of 7? to keep it working efficiently 

## Cleaning and organising columns
- Open Power Query Editor
> Home -> transform data -> remove columns -> close and apply

Can also **choose columns** as there were >170 of them and I only wanted 8
- Merge tables
- Power BI automatically picked up a relationship between the 2 tables with the column SEQN
- For the analysis I decided it was easier to merge the 2
> Home -> transform data -> Home -> Combine -> Merge queries (Full Outer) -> expand -> Close and apply

- could be any merge option as both datasets have the same number of rows and are complete

- rename the main query to 'data'

------------------------------------------------------

## PAGE 1 - DEMOGRAPHICS OF STUDY POPULATION
This page is designed to give a quick overview of some key variables in the study population.
- gender
- income
- race
- age

![Page 1 whole](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/12f503c2-665c-45df-abad-27e38a844527)
 

### GRAPH 1 - COUNT OF PARTICIPANTS BY GENDER
Stacked Bar Chart


![Gender graph](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/28a07d77-17eb-4789-a4d0-6045b79d9097)

What:
- basic chart showing split between both genders  
- created a new column Gender to convert research codes '1' and '2' to 'male' and 'female' to make easier to interpret
- [documentation](https://wwwn.cdc.gov/nchs/nhanes/2013-2014/demo_h.htm#RIAGENDR)


How:
> Create column: **Home -> New Column**      (a window for a DAX expression appears)

> DAX: **Gender = SWITCH ( TRUE (), data[RIAGENDR] = 1, "male", data[RIAGENDR] = 2, "female", "other")**

> Make graph: **Report view -> Vizualisations -> Build Visual -> Stacked bar chart**

> X-axis 'Gender' Y-axis 'Count of Gender' (drag Gender column from 'Data' into both sections)

Notes:
- sometimes the data type in the original column will need changing e.g. to a whole number to convert it
- roughly equally sized samples

### GRAPH 2 - COUNT OF PARTICIPANTS BY RACIAL GROUP
Stacked Column Chart

![Race graph](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/a29625d9-fd78-49f2-a710-57b83e934be5)

What:
- convert research numeric codes into text categories as per the [documentation](https://wwwn.cdc.gov/nchs/nhanes/2013-2014/demo_h.htm#RIDRETH3)
- make column chart X-axis - Race Y-axis - Count of Race

How:
- process as previous
> DAX: **Race = SWITCH ( TRUE(), data[RIDRETH3] = 1, "Mexican American", data[RIDRETH3] = 2, "Other Hispanic", data[RIDRETH3] = 3, "Non-Hispanic White", data[RIDRETH3] = 4, "Non-Hispanic Black", data[RIDRETH3] = 6, "Non-Hispanic Asian", data[RIDRETH3] = 7, "Other Race - Including Multi-Racial", BLANK())**

Notes:
- note there is no category 5
- the race with the highest frequency is Non-Hispanic White at 3700 participants

### GRAPH 3 - COUNT OF PARTICIPANTS BY INCOME (annual household)
Stacked Column Chart (with custom order)

![Income graph](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/07144fb1-b370-41e7-97ab-6ea9f90286b0)

What:
- convert research numberic codes into income categories as per the [documentation](https://wwwn.cdc.gov/nchs/nhanes/2013-2014/demo_h.htm#INDHHIN2)

How:
- process as previous
> DAX: **Income = SWITCH ( TRUE(), data[INDHHIN2] = 1, "0-5", data[INDHHIN2] = 2, "5-10", data[INDHHIN2] = 3, "10-15", data[INDHHIN2] = 4, "15-20", data[INDHHIN2] = 5, "20-25", data[INDHHIN2] = 6, "25-35", data[INDHHIN2] = 7, "35-45", data[INDHHIN2] = 8, "45-55", data[INDHHIN2] = 9, "55-65", data[INDHHIN2] = 10, "65-75", data[INDHHIN2] = 12, ">20", data[INDHHIN2] = 13, "<20", data[INDHHIN2] = 14, "75-100", data[INDHHIN2] = 15, ">100", BLANK())**

Notes:
- note the categories <20 and >20, I decided to order these between 15-20 and 20-25 
- because it is text, we need to set the order of the categories manually
- though the columns are in ascending order, the legend is not in the correct order, I couldn't find how to fix this

How to set a manual order for bars: 
- Create a new query (table) called 'Income Order' with 2 coloumns
- First coloumn called 'bins' with the same categories as the column 'Income' that we just created
- Second column 'income order' ranging from 1-15 in the order that is needed 
> Home -> Transform Data -> Enter Data -> close and apply

SCREENSHOT ORDER SETTING QUERY 

How to create a relationship between the 2 queries:
> Model view -> Manage relationships -> New

- based on columns
- match income order[bins] to data[Income]
- use 'Many to one cardinality'

![Manage relationships](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/95dc3f43-f7eb-4123-af8c-4bee3478201e)

Create bar chart:
> X-axis - income order[income order], Legend - data[Income], Y-axis - count of data[Income]

### GRAPH 4 - COUNT OF PARTICIPANTS BY AGE
Stacked Column Chart (Histogram) 

![Age graph](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/ffe902ac-d70f-4b39-80f7-b576cce587f5)

What:
- used the variable [age in years at screening](https://wwwn.cdc.gov/nchs/nhanes/2013-2014/demo_h.htm#RIDAGEYR)
- grouped years into appropriate intervals (5 years) and plot the frequency on the Y-axis 

How:
- as previously, create a new column with custom 'bins' which are ranges to categorise the age variable
- as age is numeric we can use a range when defining the variables 
> DAX: **Age Bins = SWITCH ( TRUE(), data[RIDAGEYR] >= 0 && data[RIDAGEYR] < 5, "0-5", ...**

- create a new query with the order of the bins and create a relationship between the 2 queries (as before)
- create visual 
  - X-axis - age order[age order], Y-axis - count of data[Age Bins], Legend - data[Age Bins]

### CARDS - MEAN, MEDIAN, INTERQUARTILE RANGE OF AGES
- Made using measures with DAX 
> Mean Age = AVERAGE(data[RIDAGEYR])

> Median Age = MEDIAN(data[RIDAGEYR])

> 75th Percentile Age = PERCENTILE.INC(data[RIDAGEYR], 0.75)

> 25th Percentile Age = PERCENTILE.INC(data[RIDAGEYR], 0.25)

> IQ Range Age = data[75th Percentile Age] - data[25th Percentile Age]

Notes:
- Outliers could be determined by (75th Percentile + (1.5 * IQR)) and (25th Percentile - (1.5 * IQR))
- I did a quick visual check of the histogram to see if the values were reasonable
- I would make a box and whisker diagram with these measures to assess the skewness/symmetry of the data
- From the histogram I would expect it to show a small positive skew


### MAKING THE DISPLAY READABLE
These are some changes I made to make the graphs more aesthetically pleasing and easy to interpret.

These can mostly be found in the 'Format your Visual' section of the 'Visualizations' pane.

- Tidied up the titles and axis labels (make central, simplify, include units where needed)
- Changed the colour theme so they all match 
- Change individual bar / column colours
- Added data labels 
- Hide the axis for any ordered bar chart as it is meaningless 
- Tooltips are on 

![Tooltip example](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/bd7c54f0-b122-40df-8253-d6900e05eda6)

### FILTERING
There are numerous ways to filter the data, but if you click on any variable on any chart on a page, all of the graphs show the filtered subset as a proportion of the total:

![Example of filtering](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/4144aff1-9700-4fb0-b98f-2c23b14e00c9)

-------------------------------------------------------------
## PAGE 2
![Page 2 whole](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/28fddcf1-5627-422f-94ea-a81490acdb71)

This page is exploring the relationships between variables (age and height, weight and height).

I chose these variables to display a range of graphs and statistical methods, as I know already the likely relationships between them (positive correlation between height and weight, height is normally distributed, positive correlation between age and height in children).


### GRAPH 1 - HEIGHT VS WEIGHT IN ADULTS
Scatter Graph

![Height v weight](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/8e908083-3bc9-4db1-a6c8-1ac81c3aea2d)

What:
- Create new columns to filter out those aged under 16 (as not fully grown) and pregnant women (which would raise their weight).
- Make scatter graph with height on x-axis and weight on y-axis

How:
> Create New Column DAX: **Weight Adult (not pregnant) = IF(data[RIDAGEYR] > 16 && data[RIDEXPRG] <> 1, data[examination.BMXWT], BLANK())**

- This means that if the conditions are met, the column will fill in with the participant's weight, if not it will be blank.
- I repeated this for Height Adult (not pregnant) so that all the data points are paired
- Change to 'Don't Summarize' on the axis drop down menu
- Plot a trend line 
> **Add further analyses to your visual -> Trend line -> On** 

Notes:
- We can see that there is a positive correlation between height and weight from the trend line (as x increases, y increases)
- There is a lot of variation in weight, this study population are from the USA, so it makes sense that a high proportion are overwight/obese
- There are also some underweight individuals, about 40kgs

### CORRELATION COEFFICIENT
- This is a really long complicated DAX formula, but Power BI has a built in quick measure to calculate it. 
> Home -> Quick measure -> correlation coefficient
> Put this measure on a card and overlay on scatter graph 

- The correlation is 0.96, which is very strongly positive!
- From the scatter graph I wouldn't expect it to be this high, but the overlap of the dots make it impossible to get an idea of the density here 

### GRAPH 2 - HEIGHT VS WEIGHT ONLY HEALTHY/OVERWEIGH INDIVIDUALS
![Height vs weight BMI filter](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/7d44b1cc-28a5-49eb-8c40-b39cf5f7e5f7)

- Out of interest I removed those from the sample with a BMI of > 30 and < 18
- Correlation coefficient improves .. suspiciously high 

### Graph 3 - Histogram of adult height, normal distribution
- create column to sort height into bins (increments of 5cm)
- set the order of the height bins by creating a new query and creating a relationship between the 2
- use measures to calculate mean and standard devation for adult height (AVERAGE()) (STDEV.P()) with FILTER function for > 16 years old
> **Mean Height Adult = CALCULATE(AVERAGE(data[examination.BMXHT]), FILTER(data, data[RIDAGEYR] > 16))

- create a new column with calculated normal distribution (NORM.DIST())
- plot a line and stacked column chart: 
    - x-axis height bin order
    - y-axis count of adult height
    - line y-axis as average of adult normal distribution for height
    - legend are height bins

becuase normal distribution is hard coded - does not change with filters

### Graph 4 
SCREENSHOT
Calculated the Z-score = (value - mean) / standard deviation
> Z-score Adult Height = (data[Height Adult] - 167.11) / 10.16

- I then removed any value with a z-score of >3 and < -3
- there were 5 adult outliers 

histogram of zscores 
- almost 2 peaks for male and female
- would separate these 

### Graph 5 - box and whisker 

