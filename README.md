# NHANES-POWERBI-ANALYTICS

**test**
> test 

## Project Overview

- this project is about getting to grips with the most commonly used graphs and statisitcal techniques
- not neccessarily what researchers would be most interested in
- start using DAX, methods, relationships, visual features
- interpretation of results

### About NHANES II
- put a link to the dataset
- inclusion, exclusion criteria

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

## Page 1 - demographics of sample population
- count of gender
- count of income
- count of race
- count of age groups 
- 
