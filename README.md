# NHANES-POWERBI-ANALYTICS

## PROJECT OVERVIEW
- First time using Power BI
- This project is about getting to grips with some common graphs and statistical techniques (not neccessarily the analysis that researchers would be most interested in)
- This is a dataset from kaggle, so I'm sure it features in many data projects already (as well as the medical literature)
- I did not look at any other interpretations of the data before doing this project, the focus was on using Power BI
- The dataset is already clean, so the data cleaning was minimal here but is a very important topic
- There are lots of other visualisations that can be imported into Power BI, but I used the basic version

### INTRO TO NHANES
- [Kaggle dataset](https://www.kaggle.com/datasets/cdc/national-health-and-nutrition-examination-survey)
- NHANES is a longitudinal health and nutrition study (using surveys, examinations, blood tests, x-rays) of around 5000 residents of the USA
- Started in 1971, data has been collected yearly since 1999
- This is the 2013-2014 dataset
- The aim is to monitor trends in the population, identify risk factors for disease, extrapolate incidence and prevalence of risk factors/diseases to the rest of the USA
- Therefore there was effort made to make it representative of the population
- More information can be found [here](https://wwwn.cdc.gov/nchs/nhanes/continuousnhanes/overview.aspx?BeginYear=2013)

### SUMMARY OF POWER BI TECHNIQUES USED
- Cleaning - remove columns, merge queries, filtering (in columns and on visualisations), changing data types, decimal places, removing outliers
- Creating columns (IF, SWITCH, FILTER) to exclude data points, re-categorise (from number to text), grouping by ranges
- Creating measures (AVERAGE, MEDIAN, PERCENTILE.INC, STDEV.P, NORM.DIST, Correlation coefficient)
- Creating relationships between tables
- Graphs: bar charts, scatter graph, line graph, histogram (using column chart), trend lines, box and whisker?
- Slicers
- Importing excel graphs into Power BI?

### STATS USED
- Mean, median, percentiles
- Standard deviation
- Normal distribution
- Symmetry / skew, interquartile range
- Z-scores
- Correlation coefficient

### SET-UP
- Downloaded datasets from kaggle, csv format
> Home -> Get Data -> text/csv -> connect -> open -> load

- Only connected 2 out of 6 to keep it working efficiently
- Rename the main query to 'data'

### CLEANING - REMOVE COLUMNS
- Open 'Power Query Editor'
> Home -> transform data -> remove columns -> close and apply

Can also **choose columns** as there were >170 of them and I only wanted 8

### MERGE TABLES
- Power BI automatically picked up a relationship between the 2 tables with the column SEQN
- For the analysis I decided it was easier to merge the 2
> Home -> transform data -> Home -> Combine -> Merge queries (Full Outer) -> expand -> Close and apply

- (Could be any merge option as both datasets have the same number of rows and are complete)


------------------------------------------------------

## PAGE 1 - DEMOGRAPHICS OF STUDY POPULATION
This page is designed to give a quick overview of some key variables in the study population.

![Page 1 whole](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/12f503c2-665c-45df-abad-27e38a844527)
 

### GRAPH 1 - COUNT OF PARTICIPANTS BY GENDER
Stacked Bar Chart

![Gender graph](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/28a07d77-17eb-4789-a4d0-6045b79d9097)

What:
- Basic chart showing split between both genders  
- Created a new column Gender to convert research codes '1' and '2' to 'male' and 'female' to make easier to interpret
- [Documentation](https://wwwn.cdc.gov/nchs/nhanes/2013-2014/demo_h.htm#RIAGENDR)


How:
> Create column: **Home -> New Column**      (a window for a DAX expression appears)

> DAX: **Gender = SWITCH ( TRUE (), data[RIAGENDR] = 1, "male", data[RIAGENDR] = 2, "female", "other")**

> Make graph: **Report view -> Vizualisations -> Build Visual -> Stacked bar chart**

> X-axis 'Gender' Y-axis 'Count of Gender' (drag Gender column from 'Data' into both sections)

Notes:
- Sometimes the data type in the original column will need changing e.g. to a whole number to convert it
- Roughly equally sized samples

### GRAPH 2 - COUNT OF PARTICIPANTS BY RACIAL GROUP
Stacked Column Chart

![Race graph](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/a29625d9-fd78-49f2-a710-57b83e934be5)

What:
- Convert research numeric codes into text categories as per the [documentation](https://wwwn.cdc.gov/nchs/nhanes/2013-2014/demo_h.htm#RIDRETH3)
- Make column chart X-axis - Race Y-axis - Count of Race

How:
- Process as previous
> DAX: **Race = SWITCH ( TRUE(), data[RIDRETH3] = 1, "Mexican American", data[RIDRETH3] = 2, "Other Hispanic", data[RIDRETH3] = 3, "Non-Hispanic White", data[RIDRETH3] = 4, "Non-Hispanic Black", data[RIDRETH3] = 6, "Non-Hispanic Asian", data[RIDRETH3] = 7, "Other Race - Including Multi-Racial", BLANK())**

Notes:
- Note there is no category 5
- The race with the highest frequency is Non-Hispanic White at 3700 participants

### GRAPH 3 - COUNT OF PARTICIPANTS BY INCOME (annual household)
Stacked Column Chart (with custom order)

![Income graph](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/07144fb1-b370-41e7-97ab-6ea9f90286b0)

What:
- Convert research numberic codes into income categories as per the [documentation](https://wwwn.cdc.gov/nchs/nhanes/2013-2014/demo_h.htm#INDHHIN2)

How:
- Process as previous
> DAX: **Income = SWITCH ( TRUE(), data[INDHHIN2] = 1, "0-5", data[INDHHIN2] = 2, "5-10", data[INDHHIN2] = 3, "10-15", data[INDHHIN2] = 4, "15-20", data[INDHHIN2] = 5, "20-25", data[INDHHIN2] = 6, "25-35", data[INDHHIN2] = 7, "35-45", data[INDHHIN2] = 8, "45-55", data[INDHHIN2] = 9, "55-65", data[INDHHIN2] = 10, "65-75", data[INDHHIN2] = 12, ">20", data[INDHHIN2] = 13, "<20", data[INDHHIN2] = 14, "75-100", data[INDHHIN2] = 15, ">100", BLANK())**

Notes:
- Note the categories <20 and >20, I decided to order these between 15-20 and 20-25 
- Because it is text, we need to set the order of the categories manually
- Though the columns are in ascending order, the legend is not in the correct order, I couldn't find how to fix this

How to set a manual order for bars: 
- Create a new query (table) called 'Income Order' with 2 coloumns
- First coloumn called 'bins' with the same categories as the column 'Income' that we just created
- Second column 'income order' ranging from 1-15 in the order that is needed 
> Home -> Transform Data -> Enter Data -> close and apply

![Income Order](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/8283bc32-ad3a-47aa-9b2f-2e4d5b63c423) 

How to create a relationship between the 2 queries:
> Model view -> Manage relationships -> New

- Based on columns
- Match income order[bins] to data[Income]
- Use 'Many to one cardinality'

![Manage relationships](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/95dc3f43-f7eb-4123-af8c-4bee3478201e)

Create bar chart:
> X-axis - income order[income order], Legend - data[Income], Y-axis - count of data[Income]

### GRAPH 4 - COUNT OF PARTICIPANTS BY AGE
Stacked Column Chart (Histogram) 

![Age graph](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/ffe902ac-d70f-4b39-80f7-b576cce587f5)

What:
- Used the variable [age in years at screening](https://wwwn.cdc.gov/nchs/nhanes/2013-2014/demo_h.htm#RIDAGEYR)
- Grouped years into appropriate intervals (5 years) and plot the frequency on the Y-axis 

How:
- As previously, create a new column with custom 'bins' which are ranges to categorise the age variable
- As age is numeric we can use a range when defining the variables 
> DAX: **Age Bins = SWITCH ( TRUE(), data[RIDAGEYR] >= 0 && data[RIDAGEYR] < 5, "0-5", ...**

- Create a new query with the order of the bins and create a relationship between the 2 queries (as before)
- Create visual 
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
- Visually from the scatter graph I wouldn't expect it to be this high, but the overlap of the dots make it impossible to get an idea of the density here 

### GRAPH 2 - HEIGHT VS WEIGHT ONLY HEALTHY/OVERWEIGH INDIVIDUALS
![Height vs weight BMI filter](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/7d44b1cc-28a5-49eb-8c40-b39cf5f7e5f7)

- Out of interest I removed those from the sample with a BMI of > 30 and < 18
- Note these are not true statistical outliers
- Correlation coefficient improves to 0.98
- There is a sharp 'line' of dots at the upper cut off point 
- Note Power BI automatically adjusts the scale on the axes

### GRAPH 3 - ADULT HEIGHT
Stacked Column Chart with Line (Histogram with Normal Distribution)
![Height histogram](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/41d5d184-81d9-406f-b242-a4a5e6509ce1)

What and How:
- Create column to sort height into bins (increments of 5cm)
- Set the order of the height bins by creating a new query and creating a relationship between the 2
- Use measures to calculate mean and standard devation for adult height (AVERAGE()) (STDEV.P()) with FILTER function for > 16 years old
> **Mean Height Adult = CALCULATE(AVERAGE(data[examination.BMXHT]), FILTER(data, data[RIDAGEYR] > 16))**

- Create a new column with calculated normal distribution (NORM.DIST())
- The NORM.DIST() function requires mean and standard deviation, which must be hard coded
- Plot a line and stacked column chart: 
    - x-axis height bin order
    - y-axis count of adult height
    - line y-axis as **average** of adult normal distribution for height
    - legend is height bins

Notes:
- [Normal Distribution in Power BI](https://community.fabric.microsoft.com/t5/Desktop/Normal-Distribution/td-p/1729978)
- We can see roughtly that the sample population height is normally distributed (as expected), however the peaks are not totally in line 
- This may be because the median is lower than the mean?? 
- Because the mean and standard deviation for normal distribution are hard coded - it does not change with gender filters
- Though this allows us to compare the gender distribution with the whole population  

![height female](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/f760c42e-8771-4cf1-b8e7-4076cbb5ad92)

- When filtered for the female sample, we can see that there is still a normal distribution centered around a lower mean height than the whole population mean height

### GRAPH 4 - Z-SCORE DISTRIBUTION
Stacked Column Charts (Histogram)
Z-scores is the range of differences between variables and the mean of the population.
0 represents the mean, the units represent the number of standard deviations from the mean in each direction, and the frequency that these occur in the sample.

![zscore all](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/26e2c6e9-2396-4dd9-a8fe-bc99ae796cb5)

Filter: Z-scores for females


![zscore female](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/991b029f-586e-4d06-ae61-2c6600ac2c38)

Filter: Z-scores for males


![zscore male](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/0f6c5f6e-44fc-416e-a149-a1213ab38440)

What + How:
- Create a new column and calculate the z-score for each height = (value - mean) / standard deviation
> Z-score Adult Height = (data[Height Adult] - 167.11) / 10.16

- Grouped these into intervals of 0.2 (z-scores)
- I then removed any value with a z-score of > 3 and < -3
- There were only **5** adult outliers
- You could limit this to 2.5 * standard devation from the mean

Notes: 
- For the whole study population, again there is roughly a normal distribution here, there are almost 2 peaks for male and female
- When filtered for gender, we can see that the mean z-score for females is less than 0, and for males is more than 0

### Graph 5 - box and whisker 

---------------------------------------------------------
## PAGE 3 - RELATIONSHIP BEWTWEEN AGE AND HEIGHT IN CHILDREN

Line Graphs
What and How:
- Plot line graph with X-axis - age in months Y-axis mean of height
- Filtered out the blank values (adults) on the Filter pane on the right of the page
- Add a 'slicer' visual to the page to toggle between male and female 

![Child Height](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/37163fa7-07f4-4ac7-bc85-63259c975596)

![example of filter](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/2aed51ce-0323-43dc-ac04-3b33f56af58f)

![Child height female](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/0cfc5b2e-1ec2-41d9-abed-173937879c72)

![child height male](https://github.com/emm-sam/NHANES-POWERBI-ANALYTICS/assets/100299675/6d10594b-1d59-436f-a311-15b726c01394)

Notes: 
- As we would expect, as age increases, height increases fairly linearly
- We can see there is some variability around the trend line, this is likely because age is measured in months here so some of the categories will only have a sample of around 10 children
- Note the plateau of mean height happens at an earlier age in females than in males, which would be expected

### FUTURE LEARNINGS 
- How to custom order the legend
- Using parameters instead of filters
- Time based graphs and trends
- Improve running speed / efficiency, visualisations were getting slow to load at the end, probably too many calculated columns 
- Removing null values, cleaning text data 
- Bookmarks, drill-down

### RESOURCES USED
- list your resources 
