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

## Page 1 - demographics of sample population
Justification: 

- count of gender
- count of income
- count of race
- count of age groups 

### Graph 1 - Count of participants by Gender
Stacked Bar Chart
SCREENSHOT OF GRAPH
- created a new column to change the research codes '1' and '2' to 'male' and 'female' to make the visualisation clearer
- sometimes the data type in the original column will need changing e.g. to a whole number 
> Create column: **Home -> New Column**      (a window for a DAX expression appears)

> DAX: **Gender = SWITCH ( TRUE (), data[RIAGENDR] = 1, "male", data[RIAGENDR] = 2, "female", "other")**

> Make graph: **Report view -> Vizualisations -> Build Visual -> Stacked column chart**

> X-axis 'Gender' Y-axis 'Count of Gender' (drag gender column from Data into both sections

### Graph 2 - Count of participants by Racial Group
Stacked Column Chart
SCREENSHOT OF GRAPH
- process as previous
> DAX: **Race = SWITCH ( TRUE(), data[RIDRETH3] = 1, "Mexican American", data[RIDRETH3] = 2, "Other Hispanic", data[RIDRETH3] = 3, "Non-Hispanic White", data[RIDRETH3] = 4, "Non-Hispanic Black", data[RIDRETH3] = 6, "Non-Hispanic Asian", data[RIDRETH3] = 7, "Other Race - Including Multi-Racial", BLANK())**

- link to the research details
- note there is no category 5

### Graph 3 - Count of participants by Income (annual household income)
Stacked Bar Chart with Custom Order
SCREENTSHOT OF GRAPH
- process as previous
> DAX: **Income = SWITCH ( TRUE(), data[INDHHIN2] = 1, "0-5", data[INDHHIN2] = 2, "5-10", data[INDHHIN2] = 3, "10-15", data[INDHHIN2] = 4, "15-20", data[INDHHIN2] = 5, "20-25", data[INDHHIN2] = 6, "25-35", data[INDHHIN2] = 7, "35-45", data[INDHHIN2] = 8, "45-55", data[INDHHIN2] = 9, "55-65", data[INDHHIN2] = 10, "65-75", data[INDHHIN2] = 12, ">20", data[INDHHIN2] = 13, "<20", data[INDHHIN2] = 14, "75-100", data[INDHHIN2] = 15, ">100", BLANK())**

- these categories were decided by the research group
- note the categories <20 and >20, I decided to order these between 15-20 and 20-25 
- because it is text, we need to set the order of the categories manually

To set a manual order for bars: 
- Create a new query (table) called 'Income Order' with 2 coloumns. 
- First coloumn called 'Bins' with the same categories as the column 'Income' that we just created
- Second column 'income order' ranging from 1-15 in the order that is needed 
> Home -> Transform Data -> Enter Data -> close and apply

> Model view -> Manage relationships -> New

- Create a relationship between the 2 tables based on columns
- match income order[bins] to data[Income]
- use Many to one cardinality

SCREENSHOT?

Create bar chart
> X-axis income order[income order], legend data[Income], Y-axis count of data[Income]

### Graph 4 - Count of participants by Age
Stacked column chart (Manual histogram) 
SCREENSHOT
- as previously, create a new column with custom 'bins' which are ranges to categorise the age variable
> DAX: **Age Bins = SWITCH ( TRUE(), data[RIDAGEYR] >= 0 && data[RIDAGEYR] < 5, "0-5", ...**

- create a new table with the order of the bins as before and create a relationship between the 2 tables
- create visual 
  - X-axis age order[age order], Y-axis count of data[Age Bins], Legend data[Age Bins]

### Cards - Mean, Median, Interquartile Range of Ages
- Made using measures with DAX
> Mean Age = AVERAGE(data[RIDAGEYR])

> Median Age = MEDIAN(data[RIDAGEYR])

> 75th Percentile Age = PERCENTILE.INC(data[RIDAGEYR], 0.75)

> 25th Percentile Age = PERCENTILE.INC(data[RIDAGEYR], 0.25)

> IQ Range Age = data[75th Percentile Age] - data[25th Percentile Age]

Comments:
- Outliers could be assessed with (75th Percentile + 1.5 * IQR), or (25th Percentile - 1.5 * IQR)
- I did a quick visual check of the histogram to see if the values were reasonable.
- I would make a box and whisker diagram with these measures to assess the skewness/symmetry of the data.
- From the histogram I would expect it to show a small positive skew. 


### Making the display readable
Some things that I changed to make the visualisation more aesthetically pleasing and easy to interpret.

These can mostly be found in the 'Format your Visual' section of the 'Visualizations' pane

- Tidied up the titles and axis labels (make central, simplify, include units where needed)
- Changed the colour theme so they all match 
- Change individual bar / column colours
- Added data labels 
- Hide the axis for any ordered bar chart as it is meaningless 
- Tooltips are on 


## Page 2
SCREENSHOT
This page is exploring the relationships between examination variables (age and height, weight and height).

I chose these because there should be a positive correlation between height and weight and height is usually normally distributed in a population. 


### Graph 1 - Scatter Graph of height vs weight 
SCREENSHOT


- To start I created new columns to filter out those aged under 16 (as not fully grown) and pregnant women (which would raise their weight). 
> Create New Column DAX: **Weight Adult (not pregnant) = IF(data[RIDAGEYR] > 16 && data[RIDEXPRG] <> 1, data[examination.BMXWT], BLANK())**

- This means that if the conditions are met, the column will fill in with the participant's weight, if not it will be blank.
- I repeated this for Height Adult (not pregnant)

- Make scatter graph with height on x-axis and weight on y-axis
- Change to 'Don't Summarize' on the axis drop down menu
- plot a trend line 
> **Add further analyses to your visual -> Trend line -> On** 

### Correlation co-efficient for Graph 1
- this is a really long complicated DAX formula, but Power BI has a built in quick measure to calculate it. 
> Home -> Quick measure -> correlation coefficient 

### Graph 2 - Scatter graph with rough outliers removed 
- removed those from the sample with a BMI of > 30
- correlation coefficient improves .. suspiciously high 

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

Calculated the Z-score = (value - mean) / standard deviation
> Z-score Adult Height = (data[Height Adult] - 167.11) / 10.16

- I then removed any value with a z-score of >3 and < -3
- there were 5 adult outliers 


