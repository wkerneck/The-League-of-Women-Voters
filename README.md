# 2016-0509 MSDS 6304-401 Week 8 Case Study
Chris Woodward Bill Kerneckel  
July 10, 2016  

<br>
                                                               


#### Problem Statement


The League of Women Voters asked PhD and Masters’ students enrolled in a statistical
consulting course in the Department of Statistical Science at SMU to help them
perform an experiment to determine whether sending either a postcard or a voter’s
guide (flyer) to potential voters increased the percent who actually voted in the 2014 election. The LWV was particularly interested in the effect on voting behavior among young and Hispanic voters, since this group tended to be less likely to vote in past elections.

The project spanned three semesters. In the spring of 2014, students estimated the
sample size needed for the study and developed the experimental design and
sampling methodology. In the fall of 2014, the data were gathered and cleaned, and inthe spring of 2015, the data were analyzed.

The sample consisted of 24,000 randomly selected individuals from a population of
531,735. The population was low-propensity voters, determined ahead of the 2014
election. A low propensity voter is someone who has only voted in 0 or 1 of the last
three elections. The sample was randomly partitioned into three treatment groups.

- 8000 received a postcard reminding them to vote
- 8000 received a flyer with a reminder and voting instructions
- 8000 did not receive any mailing. These are the control group.

After the 2014 election, information regarding voter participation was collected for all voters.

When the data were analyzed, it was found that 10% of the low propensity voters who
had received the flyer voted, approximately 12% of those who received the postcard
voted, and approximately 23% of the control group voted.

The LWV was not happy with these results, as they had expected that some sort of
reminder, whether a flyer or a postcard, would increase the chances of voting. The LWV asked the students to go back and examine the data. And guess what? There was a
problem. 

****************************

#### Assignment

You are to figure out the problem. There is a good (albeit subtle) reason that the
expectations do not match the results, and it can be found in the data. The reason has something to do with the topic for Unit 8, which is statistical sampling.

The LWV data is supplied to you in a CSV file and a SAS file. All that are required to figure out the problem are simple descriptive statistics and graphics that you have already encountered in the program. You may use either R or SAS to examine the data. You may also work in groups, as long as there are no more than three people in a group.

All you are to worry about is finding the problem itself. Do not worry about the remedy for the problem. It is somewhat complicated, and it will be discussed in the live session for Unit 10.

Your deliverable is a Word or Rmd document containing a well-written explanation for
the problem, along with any supporting graphics and tables. Your document should be
no more than two pages.

We will discuss the case study in Unit 10 live session.

****************************
#### Setting your working directory

In order for the analysis of thedatasets you must set your working directory to the following:


```r
setwd("/Users/wkerneck/desktop/Voter_Data/")
```

****************************

#### Table of Contents

- 1.0  [Import data](#id-section1)
- 1.1  [Alphabetic List of Variables and Attributes](#id-section2)
- 1.2  [Explorative Data Analysis (EDA)](#id-section3)
- 2.0 [Conclusion and Summary](#id-section4) 
- 3.0 [Acknowledgements](#id-section5) 

****************************
 
<div id='id-section1'/>
####  1.0 Import data

We have downloaded the data in a .CSV format. The .CSV file (LWV_Data.csv) was saved to the root of the project folder.


```r
ImportedAsIsData <- read.table("LWV_Data.csv", header=TRUE, sep=",", na.strings=c("NA", "NULL"))

ImportedCleanedData <- ImportedAsIsData[complete.cases(ImportedAsIsData),]  #Removing all "NA" values from the dataset
```

After we have removed the NA's from the dataset we now have 24000 observations.


```r
dim(ImportedCleanedData)
```

```
## [1] 24000    27
```

```r
head(ImportedCleanedData, n=3)
```

```
##     VOTED2014 Young.Hispanic.Status ID.Number Voter.Status Voted.11.2012
## 39          0               non_y_h      5461            A             1
## 51          0               non_y_h      6832            A             0
## 217         0           non_y_non_h     16298            A             0
##     Voted.Gen..Elec..09.2010 Voted.Gen..Elec..07.2008
## 39                         0                        0
## 51                         0                        0
## 217                        0                        1
##     Number.General.Elections Hispanic.Surname Young.Voter Eligible.2012
## 39                         1                1           0             1
## 51                         0                1           0             1
## 217                        1                0           0             1
##     Eligible.2010 Eligible.2008 Young.in.2012 Young.in.2010 Young.in.2008
## 39              1             1             0             0             0
## 51              1             1             0             0             0
## 217             1             1             0             0             0
##       Voter.Category             type    ID control post flyer LOWPROP
## 39      Old Hispanic     Non_y_h_POST  5461       0    1     0       1
## 51      Old Hispanic     Non_y_h_POST  6832       0    1     0       1
## 217 Old Not Hispanic Non_y_non_h_POST 16298       0    1     0       1
##           city   zip U_S__CONGRESS byear
## 39  CARROLLTON 75007            24  1937
## 51  CARROLLTON 75006            24  1911
## 217    GARLAND 75042            32  1922
```

****************************

<div id='id-section2'/>
####  1.1 List of variables contained in the dataset.


```r
str(ImportedCleanedData)
```

```
## 'data.frame':	24000 obs. of  27 variables:
##  $ VOTED2014               : int  0 0 0 1 0 0 0 0 1 0 ...
##  $ Young.Hispanic.Status   : Factor w/ 4 levels "non_y_h","non_y_non_h",..: 1 1 2 1 2 2 2 2 2 2 ...
##  $ ID.Number               : int  5461 6832 16298 20802 23641 23821 25164 27016 28260 28523 ...
##  $ Voter.Status            : Factor w/ 1 level "A": 1 1 1 1 1 1 1 1 1 1 ...
##  $ Voted.11.2012           : int  1 0 0 0 0 0 0 0 0 0 ...
##  $ Voted.Gen..Elec..09.2010: int  0 0 0 0 1 0 0 1 0 0 ...
##  $ Voted.Gen..Elec..07.2008: int  0 0 1 1 0 1 1 0 0 0 ...
##  $ Number.General.Elections: int  1 0 1 1 1 1 1 1 0 0 ...
##  $ Hispanic.Surname        : int  1 1 0 1 0 0 0 0 0 0 ...
##  $ Young.Voter             : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Eligible.2012           : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ Eligible.2010           : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ Eligible.2008           : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ Young.in.2012           : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Young.in.2010           : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Young.in.2008           : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Voter.Category          : Factor w/ 4 levels "Old Hispanic",..: 1 1 2 1 2 2 2 2 2 2 ...
##  $ type                    : Factor w/ 13 levels "","Non_y_h_CONTROL",..: 4 4 7 4 7 7 7 7 7 7 ...
##  $ ID                      : int  5461 6832 16298 20802 23641 23821 25164 27016 28260 28523 ...
##  $ control                 : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ post                    : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ flyer                   : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ LOWPROP                 : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ city                    : Factor w/ 28 levels "","ADDISON","BALCH SPRINGS",..: 4 4 12 12 12 12 12 12 12 12 ...
##  $ zip                     : int  75007 75006 75042 75040 75041 75041 75043 75040 75040 75040 ...
##  $ U_S__CONGRESS           : int  24 24 32 32 32 32 32 32 32 32 ...
##  $ byear                   : int  1937 1911 1922 1938 1911 1927 1926 1928 1944 1931 ...
```

****************************

<div id='id-section3'/>
####  1.2 Explorative Data Anaylsis (EDA)

Let's have a look and see if there are any null values left in the dataset.


```r
voter_data_null_count <- sapply(ImportedCleanedData, function(x) sum(is.na(x)))
voter_data_null_count
```

```
##                VOTED2014    Young.Hispanic.Status                ID.Number 
##                        0                        0                        0 
##             Voter.Status            Voted.11.2012 Voted.Gen..Elec..09.2010 
##                        0                        0                        0 
## Voted.Gen..Elec..07.2008 Number.General.Elections         Hispanic.Surname 
##                        0                        0                        0 
##              Young.Voter            Eligible.2012            Eligible.2010 
##                        0                        0                        0 
##            Eligible.2008            Young.in.2012            Young.in.2010 
##                        0                        0                        0 
##            Young.in.2008           Voter.Category                     type 
##                        0                        0                        0 
##                       ID                  control                     post 
##                        0                        0                        0 
##                    flyer                  LOWPROP                     city 
##                        0                        0                        0 
##                      zip            U_S__CONGRESS                    byear 
##                        0                        0                        0
```

Ok, we just verified all NA values have been removed from the data set. Now we have identified the control population.

****************************
<div id='id-section3'/>
#### 2.0 Conclusion and Summary




****************************
<div id='id-section4'/>
#### Acknowledgements

