---
title: "Midterm 1"
author: "Richard J. Dean"
date: "2021-02-02"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Be sure to **add your name** to the author header above. You may use any resources to answer these questions (including each other), but you may not post questions to Open Stacks or external help sites. There are 12 total questions.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

This exam is due by **12:00p on Thursday, January 28**.  

## Load the tidyverse
If you plan to use any other libraries to complete this assignment then you should load them here.

```r
library("tidyverse")
library("janitor")
```

## Questions
**1. (2 points) Briefly explain how R, RStudio, and GitHub work together to make work flows in data science transparent and repeatable. What is the advantage of using RMarkdown in this context?**  
R is an open source, scripting language. RStudio is a GUI that is used to interact with R to make it more user friendly. R is necessary for Rstudio to work. Github is a file storage and management site used by programmers. Programmers from around the world upload code to repositories and make it publicly available for everyone to see and/or download and manipulate themselves. This allows people to transparently see the steps someone took to manipulate data to get the results they observed so that others can follow it and repeat their process/verify the correctness of their results. The advantage of using RMarkdown is that it allows us to embed code in annotated chunks that are easy to read for other programmers, show the results of analyses, and display graphical output all in one easy to read file. It is a very convenient way of making a report which can be output to html or pdf to be pushed to Github for everyone to see.

**2. (2 points) What are the three types of `data structures` that we have discussed? Why are we using data frames for BIS 15L?**
Vectors, Data Matrices and Data frames. We are using data frames because one can store data from many different classes within a data frame, and they're the most common way to organize data within R. 

In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

**3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.**

```r
elephants<-readr::read_csv("data/ElephantsMF.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   Age = col_double(),
##   Height = col_double(),
##   Sex = col_character()
## )
```

**4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.**

```r
elephants<-rename(elephants, age="Age", height="Height", sex="Sex")
```

```r
elephants$sex <- as.factor(elephants$sex)
```
**5. (2 points) How many male and female elephants are represented in the data?**

```r
count(elephants,sex,sort=T)
```

```
## # A tibble: 2 x 2
##   sex       n
##   <fct> <int>
## 1 F       150
## 2 M       138
```
150 females ad 138 males
**6. (2 points) What is the average age all elephants in the data?**

```r
elephants %>% 
  summarize(mean.age=mean(age),.groups='keep')
```

```
## # A tibble: 1 x 1
##   mean.age
##      <dbl>
## 1     11.0
```
**7. (2 points) How does the average age and height of elephants compare by sex?**

```r
elephants %>% 
  group_by(sex) %>% 
  summarize(mean.age=mean(age),
            mean.height=mean(height), .groups='keep')
```

```
## # A tibble: 2 x 3
## # Groups:   sex [2]
##   sex   mean.age mean.height
##   <fct>    <dbl>       <dbl>
## 1 F        12.8         190.
## 2 M         8.95        185.
```
males are shorter and younger on average than females.
**8. (2 points) How does the average height of elephants compare by sex for individuals over 25 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.**

```r
elephants %>% 
  filter(age>25) %>% 
  group_by(sex) %>% 
  summarize(mean.height=mean(height),
            min.height=min(height),
            max.height=max(height),
            n=n(), .groups='keep')
```

```
## # A tibble: 2 x 5
## # Groups:   sex [2]
##   sex   mean.height min.height max.height     n
##   <fct>       <dbl>      <dbl>      <dbl> <int>
## 1 F            233.       206.       278.    25
## 2 M            273.       237.       304.     8
```

For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

**9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.**

```r
cats<-readr::read_csv("data/IvindoData_DryadVersion.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   .default = col_double(),
##   HuntCat = col_character(),
##   LandUse = col_character()
## )
## i Use `spec()` for the full column specifications.
```

```r
cats<-janitor::clean_names(cats)
names(cats)
```

```
##  [1] "transect_id"              "distance"                
##  [3] "hunt_cat"                 "num_households"          
##  [5] "land_use"                 "veg_rich"                
##  [7] "veg_stems"                "veg_liana"               
##  [9] "veg_dbh"                  "veg_canopy"              
## [11] "veg_understory"           "ra_apes"                 
## [13] "ra_birds"                 "ra_elephant"             
## [15] "ra_monkeys"               "ra_rodent"               
## [17] "ra_ungulate"              "rich_all_species"        
## [19] "evenness_all_species"     "diversity_all_species"   
## [21] "rich_bird_species"        "evenness_bird_species"   
## [23] "diversity_bird_species"   "rich_mammal_species"     
## [25] "evenness_mammal_species"  "diversity_mammal_species"
```

```r
summary(cats)
```

```
##   transect_id       distance        hunt_cat         num_households 
##  Min.   : 1.00   Min.   : 2.700   Length:24          Min.   :13.00  
##  1st Qu.: 5.75   1st Qu.: 5.668   Class :character   1st Qu.:24.75  
##  Median :14.50   Median : 9.720   Mode  :character   Median :29.00  
##  Mean   :13.50   Mean   :11.879                      Mean   :37.88  
##  3rd Qu.:20.25   3rd Qu.:17.683                      3rd Qu.:54.00  
##  Max.   :27.00   Max.   :26.760                      Max.   :73.00  
##    land_use            veg_rich       veg_stems       veg_liana     
##  Length:24          Min.   :10.88   Min.   :23.44   Min.   : 4.750  
##  Class :character   1st Qu.:13.10   1st Qu.:28.69   1st Qu.: 9.033  
##  Mode  :character   Median :14.94   Median :32.45   Median :11.940  
##                     Mean   :14.83   Mean   :32.80   Mean   :11.040  
##                     3rd Qu.:16.54   3rd Qu.:37.08   3rd Qu.:13.250  
##                     Max.   :18.75   Max.   :47.56   Max.   :16.380  
##     veg_dbh        veg_canopy    veg_understory     ra_apes      
##  Min.   :28.45   Min.   :2.500   Min.   :2.380   Min.   : 0.000  
##  1st Qu.:40.65   1st Qu.:3.250   1st Qu.:2.875   1st Qu.: 0.000  
##  Median :43.90   Median :3.430   Median :3.000   Median : 0.485  
##  Mean   :46.09   Mean   :3.469   Mean   :3.020   Mean   : 2.045  
##  3rd Qu.:50.58   3rd Qu.:3.750   3rd Qu.:3.167   3rd Qu.: 3.815  
##  Max.   :76.48   Max.   :4.000   Max.   :3.880   Max.   :12.930  
##     ra_birds      ra_elephant       ra_monkeys      ra_rodent    
##  Min.   :31.56   Min.   :0.0000   Min.   : 5.84   Min.   :1.060  
##  1st Qu.:52.51   1st Qu.:0.0000   1st Qu.:22.70   1st Qu.:2.047  
##  Median :57.90   Median :0.3600   Median :31.74   Median :3.230  
##  Mean   :58.64   Mean   :0.5450   Mean   :31.30   Mean   :3.278  
##  3rd Qu.:68.17   3rd Qu.:0.8925   3rd Qu.:39.88   3rd Qu.:4.093  
##  Max.   :85.03   Max.   :2.3000   Max.   :54.12   Max.   :6.310  
##   ra_ungulate     rich_all_species evenness_all_species diversity_all_species
##  Min.   : 0.000   Min.   :15.00    Min.   :0.6680       Min.   :1.966        
##  1st Qu.: 1.232   1st Qu.:19.00    1st Qu.:0.7542       1st Qu.:2.248        
##  Median : 2.545   Median :20.00    Median :0.7760       Median :2.316        
##  Mean   : 4.166   Mean   :20.21    Mean   :0.7699       Mean   :2.310        
##  3rd Qu.: 5.157   3rd Qu.:22.00    3rd Qu.:0.8083       3rd Qu.:2.429        
##  Max.   :13.860   Max.   :24.00    Max.   :0.8330       Max.   :2.566        
##  rich_bird_species evenness_bird_species diversity_bird_species
##  Min.   : 8.00     Min.   :0.5590        Min.   :1.162         
##  1st Qu.:10.00     1st Qu.:0.6825        1st Qu.:1.603         
##  Median :11.00     Median :0.7220        Median :1.680         
##  Mean   :10.33     Mean   :0.7137        Mean   :1.661         
##  3rd Qu.:11.00     3rd Qu.:0.7722        3rd Qu.:1.784         
##  Max.   :13.00     Max.   :0.8240        Max.   :2.008         
##  rich_mammal_species evenness_mammal_species diversity_mammal_species
##  Min.   : 6.000      Min.   :0.6190          Min.   :1.378           
##  1st Qu.: 9.000      1st Qu.:0.7073          1st Qu.:1.567           
##  Median :10.000      Median :0.7390          Median :1.699           
##  Mean   : 9.875      Mean   :0.7477          Mean   :1.698           
##  3rd Qu.:11.000      3rd Qu.:0.7847          3rd Qu.:1.815           
##  Max.   :12.000      Max.   :0.8610          Max.   :2.065
```

```r
cats$hunt_cat<-as.factor(cats$hunt_cat)
is.factor(cats$hunt_cat)
```

```
## [1] TRUE
```

```r
cats$land_use<-as.factor(cats$land_use)
```
**10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?**

```r
cats %>% 
  filter(hunt_cat=="Moderate" | hunt_cat=="High") %>% 
  group_by(hunt_cat) %>% 
  summarize(avg.bird.diversity=mean(diversity_bird_species),
            avg.mammal.diversity=mean(diversity_mammal_species),.groups='keep')
```

```
## # A tibble: 2 x 3
## # Groups:   hunt_cat [2]
##   hunt_cat avg.bird.diversity avg.mammal.diversity
##   <fct>                 <dbl>                <dbl>
## 1 High                   1.66                 1.74
## 2 Moderate               1.62                 1.68
```
**11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 5km from a village to sites that are greater than 20km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.**  

```r
close.villages<-cats %>% 
  filter(distance<5)
far.villages<-cats %>% 
  filter(distance>20)
```

```r
close.villages %>% 
  summarize(across(starts_with("ra"),mean,na.rm=T),.groups='keep')
```

```
## # A tibble: 1 x 6
##   ra_apes ra_birds ra_elephant ra_monkeys ra_rodent ra_ungulate
##     <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1    0.08     70.4      0.0967       24.1      3.66        1.59
```

```r
far.villages %>% 
  summarize(across(starts_with("ra"),mean,na.rm=T),.groups='keep')
```

```
## # A tibble: 1 x 6
##   ra_apes ra_birds ra_elephant ra_monkeys ra_rodent ra_ungulate
##     <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1    7.21     44.5       0.557       40.1      2.68        4.98
```

It appears that the diversity of apes, elephants, monkeys, and ungulate are increased at >20km distance, but birds and rodents are increased at <5km distance from a village.
**12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`**

```r
cats %>% 
  filter(!land_use=="Neither") %>% 
  group_by(land_use) %>% 
  summarize(mean.all.div=mean(diversity_all_species), .groups='keep')
```

```
## # A tibble: 2 x 2
## # Groups:   land_use [2]
##   land_use mean.all.div
##   <fct>           <dbl>
## 1 Logging          2.23
## 2 Park             2.43
```
