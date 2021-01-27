---
title: "Lab 6 Homework"
author: "Richard J. Dean"
date: "2021-01-26"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(skimr)
```

For this assignment we are going to work with a large data set from the [United Nations Food and Agriculture Organization](http://www.fao.org/about/en/) on world fisheries. These data are pretty wild, so we need to do some cleaning. First, load the data.  

Load the data `FAO_1950to2012_111914.csv` as a new object titled `fisheries`.

```r
fisheries<-readr::read_csv("data/FAO_1950to2012_111914.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   .default = col_character(),
##   `ISSCAAP group#` = col_double(),
##   `FAO major fishing area` = col_double()
## )
## i Use `spec()` for the full column specifications.
```

1. Do an exploratory analysis of the data (your choice). What are the names of the variables, what are the dimensions, are there any NA's, what are the classes of the variables?  

```r
dim(fisheries)
```

```
## [1] 17692    71
```

```r
names(fisheries)
```

```
##  [1] "Country"                 "Common name"            
##  [3] "ISSCAAP group#"          "ISSCAAP taxonomic group"
##  [5] "ASFIS species#"          "ASFIS species name"     
##  [7] "FAO major fishing area"  "Measure"                
##  [9] "1950"                    "1951"                   
## [11] "1952"                    "1953"                   
## [13] "1954"                    "1955"                   
## [15] "1956"                    "1957"                   
## [17] "1958"                    "1959"                   
## [19] "1960"                    "1961"                   
## [21] "1962"                    "1963"                   
## [23] "1964"                    "1965"                   
## [25] "1966"                    "1967"                   
## [27] "1968"                    "1969"                   
## [29] "1970"                    "1971"                   
## [31] "1972"                    "1973"                   
## [33] "1974"                    "1975"                   
## [35] "1976"                    "1977"                   
## [37] "1978"                    "1979"                   
## [39] "1980"                    "1981"                   
## [41] "1982"                    "1983"                   
## [43] "1984"                    "1985"                   
## [45] "1986"                    "1987"                   
## [47] "1988"                    "1989"                   
## [49] "1990"                    "1991"                   
## [51] "1992"                    "1993"                   
## [53] "1994"                    "1995"                   
## [55] "1996"                    "1997"                   
## [57] "1998"                    "1999"                   
## [59] "2000"                    "2001"                   
## [61] "2002"                    "2003"                   
## [63] "2004"                    "2005"                   
## [65] "2006"                    "2007"                   
## [67] "2008"                    "2009"                   
## [69] "2010"                    "2011"                   
## [71] "2012"
```

```r
anyNA(fisheries)
```

```
## [1] TRUE
```

```r
summary(fisheries)
```

```
##    Country          Common name        ISSCAAP group#  ISSCAAP taxonomic group
##  Length:17692       Length:17692       Min.   :11.00   Length:17692           
##  Class :character   Class :character   1st Qu.:33.00   Class :character       
##  Mode  :character   Mode  :character   Median :36.00   Mode  :character       
##                                        Mean   :37.38                          
##                                        3rd Qu.:38.00                          
##                                        Max.   :77.00                          
##  ASFIS species#     ASFIS species name FAO major fishing area
##  Length:17692       Length:17692       Min.   :18.00         
##  Class :character   Class :character   1st Qu.:31.00         
##  Mode  :character   Mode  :character   Median :37.00         
##                                        Mean   :45.34         
##                                        3rd Qu.:57.00         
##                                        Max.   :88.00         
##    Measure              1950               1951               1952          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1953               1954               1955               1956          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1957               1958               1959               1960          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1961               1962               1963               1964          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1965               1966               1967               1968          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1969               1970               1971               1972          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1973               1974               1975               1976          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1977               1978               1979               1980          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1981               1982               1983               1984          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1985               1986               1987               1988          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1989               1990               1991               1992          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1993               1994               1995               1996          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      1997               1998               1999               2000          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      2001               2002               2003               2004          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      2005               2006               2007               2008          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##      2009               2010               2011               2012          
##  Length:17692       Length:17692       Length:17692       Length:17692      
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
## 
```

```r
t(t(sapply(fisheries,class)))
```

```
##                         [,1]       
## Country                 "character"
## Common name             "character"
## ISSCAAP group#          "numeric"  
## ISSCAAP taxonomic group "character"
## ASFIS species#          "character"
## ASFIS species name      "character"
## FAO major fishing area  "numeric"  
## Measure                 "character"
## 1950                    "character"
## 1951                    "character"
## 1952                    "character"
## 1953                    "character"
## 1954                    "character"
## 1955                    "character"
## 1956                    "character"
## 1957                    "character"
## 1958                    "character"
## 1959                    "character"
## 1960                    "character"
## 1961                    "character"
## 1962                    "character"
## 1963                    "character"
## 1964                    "character"
## 1965                    "character"
## 1966                    "character"
## 1967                    "character"
## 1968                    "character"
## 1969                    "character"
## 1970                    "character"
## 1971                    "character"
## 1972                    "character"
## 1973                    "character"
## 1974                    "character"
## 1975                    "character"
## 1976                    "character"
## 1977                    "character"
## 1978                    "character"
## 1979                    "character"
## 1980                    "character"
## 1981                    "character"
## 1982                    "character"
## 1983                    "character"
## 1984                    "character"
## 1985                    "character"
## 1986                    "character"
## 1987                    "character"
## 1988                    "character"
## 1989                    "character"
## 1990                    "character"
## 1991                    "character"
## 1992                    "character"
## 1993                    "character"
## 1994                    "character"
## 1995                    "character"
## 1996                    "character"
## 1997                    "character"
## 1998                    "character"
## 1999                    "character"
## 2000                    "character"
## 2001                    "character"
## 2002                    "character"
## 2003                    "character"
## 2004                    "character"
## 2005                    "character"
## 2006                    "character"
## 2007                    "character"
## 2008                    "character"
## 2009                    "character"
## 2010                    "character"
## 2011                    "character"
## 2012                    "character"
```

```r
skim(fisheries)
```


Table: Data summary

|                         |          |
|:------------------------|:---------|
|Name                     |fisheries |
|Number of rows           |17692     |
|Number of columns        |71        |
|_______________________  |          |
|Column type frequency:   |          |
|character                |69        |
|numeric                  |2         |
|________________________ |          |
|Group variables          |None      |


**Variable type: character**

|skim_variable           | n_missing| complete_rate| min| max| empty| n_unique| whitespace|
|:-----------------------|---------:|-------------:|---:|---:|-----:|--------:|----------:|
|Country                 |         0|          1.00|   4|  25|     0|      204|          0|
|Common name             |         0|          1.00|   3|  30|     0|     1553|          0|
|ISSCAAP taxonomic group |         0|          1.00|   5|  36|     0|       30|          0|
|ASFIS species#          |         0|          1.00|  10|  13|     0|     1553|          0|
|ASFIS species name      |         0|          1.00|   6|  32|     0|     1548|          0|
|Measure                 |         0|          1.00|  17|  17|     0|        1|          0|
|1950                    |     15561|          0.12|   1|   6|     0|      513|          0|
|1951                    |     15550|          0.12|   1|   7|     0|      536|          0|
|1952                    |     15501|          0.12|   1|   7|     0|      553|          0|
|1953                    |     15439|          0.13|   1|   6|     0|      570|          0|
|1954                    |     15417|          0.13|   1|   7|     0|      588|          0|
|1955                    |     15382|          0.13|   1|   7|     0|      589|          0|
|1956                    |     15331|          0.13|   1|   7|     0|      633|          0|
|1957                    |     15253|          0.14|   1|   7|     0|      627|          0|
|1958                    |     15138|          0.14|   1|   6|     0|      643|          0|
|1959                    |     15110|          0.15|   1|   7|     0|      641|          0|
|1960                    |     15016|          0.15|   1|   7|     0|      673|          0|
|1961                    |     14922|          0.16|   1|   8|     0|      713|          0|
|1962                    |     14801|          0.16|   1|   8|     0|      729|          0|
|1963                    |     14707|          0.17|   1|   8|     0|      760|          0|
|1964                    |     14349|          0.19|   1|   8|     0|      759|          0|
|1965                    |     14241|          0.20|   1|   8|     0|      798|          0|
|1966                    |     14187|          0.20|   1|   8|     0|      801|          0|
|1967                    |     14047|          0.21|   1|   8|     0|      815|          0|
|1968                    |     13963|          0.21|   1|   8|     0|      829|          0|
|1969                    |     13920|          0.21|   1|   8|     0|      853|          0|
|1970                    |     13113|          0.26|   1|   8|     0|     1225|          0|
|1971                    |     12925|          0.27|   1|   8|     0|     1326|          0|
|1972                    |     12749|          0.28|   1|   8|     0|     1372|          0|
|1973                    |     12673|          0.28|   1|   8|     0|     1432|          0|
|1974                    |     12583|          0.29|   1|   8|     0|     2601|          0|
|1975                    |     12333|          0.30|   1|   8|     0|     2767|          0|
|1976                    |     12177|          0.31|   1|   8|     0|     2804|          0|
|1977                    |     12014|          0.32|   1|   8|     0|     2867|          0|
|1978                    |     11847|          0.33|   1|   8|     0|     2901|          0|
|1979                    |     11820|          0.33|   1|   8|     0|     2932|          0|
|1980                    |     11747|          0.34|   1|   8|     0|     2956|          0|
|1981                    |     11713|          0.34|   1|   8|     0|     2996|          0|
|1982                    |     11558|          0.35|   1|   9|     0|     3030|          0|
|1983                    |     11453|          0.35|   1|   8|     0|     3031|          0|
|1984                    |     11309|          0.36|   1|   8|     0|     3076|          0|
|1985                    |     11212|          0.37|   1|   8|     0|     3161|          0|
|1986                    |     11086|          0.37|   1|   8|     0|     3242|          0|
|1987                    |     10930|          0.38|   1|   8|     0|     3255|          0|
|1988                    |     10493|          0.41|   1|   8|     0|     3435|          0|
|1989                    |     10435|          0.41|   1|   8|     0|     3425|          0|
|1990                    |     10293|          0.42|   1|   8|     0|     3446|          0|
|1991                    |     10364|          0.41|   1|   8|     0|     3420|          0|
|1992                    |     10435|          0.41|   1|   8|     0|     3322|          0|
|1993                    |     10522|          0.41|   1|   8|     0|     3313|          0|
|1994                    |     10400|          0.41|   1|   8|     0|     3313|          0|
|1995                    |     10148|          0.43|   1|   8|     0|     3329|          0|
|1996                    |      9990|          0.44|   1|   7|     0|     3358|          0|
|1997                    |      9773|          0.45|   1|   9|     0|     3393|          0|
|1998                    |      9579|          0.46|   1|   9|     0|     3399|          0|
|1999                    |      9265|          0.48|   1|   9|     0|     3428|          0|
|2000                    |      8899|          0.50|   1|   9|     0|     3481|          0|
|2001                    |      8646|          0.51|   1|   9|     0|     3490|          0|
|2002                    |      8590|          0.51|   1|   9|     0|     3507|          0|
|2003                    |      8383|          0.53|   1|   9|     0|     3482|          0|
|2004                    |      7977|          0.55|   1|   9|     0|     3505|          0|
|2005                    |      7822|          0.56|   1|   9|     0|     3532|          0|
|2006                    |      7699|          0.56|   1|   9|     0|     3565|          0|
|2007                    |      7589|          0.57|   1|   8|     0|     3551|          0|
|2008                    |      7667|          0.57|   1|   8|     0|     3537|          0|
|2009                    |      7573|          0.57|   1|   8|     0|     3572|          0|
|2010                    |      7499|          0.58|   1|   8|     0|     3590|          0|
|2011                    |      7371|          0.58|   1|   8|     0|     3618|          0|
|2012                    |      7336|          0.59|   1|   8|     0|     3609|          0|


**Variable type: numeric**

|skim_variable          | n_missing| complete_rate|  mean|    sd| p0| p25| p50| p75| p100|hist  |
|:----------------------|---------:|-------------:|-----:|-----:|--:|---:|---:|---:|----:|:-----|
|ISSCAAP group#         |         0|             1| 37.38|  7.88| 11|  33|  36|  38|   77|▁▇▂▁▁ |
|FAO major fishing area |         0|             1| 45.34| 18.33| 18|  31|  37|  57|   88|▇▇▆▃▃ |
The names of the variables are: Country, Common name, ISSCAAP group#, ISSCAAP taxonoimic group, ASFIS species#, ASFIS species name, FAO major fishing area, Measure, followed by year numbers 1950-2012. 
The dimensions are 17692x71
There are NA's in the data
The Classes of the variable are 69 Character with 2 Numeric
2. Use `janitor` to rename the columns and make them easier to use. As part of this cleaning step, change `country`, `isscaap_group_number`, `asfis_species_number`, and `fao_major_fishing_area` to data class factor. 

```r
fisheries<-janitor::clean_names(fisheries)
```


```r
fisheries$country <- as.factor(fisheries$country)
fisheries$isscaap_group_number <- as.factor(fisheries$isscaap_group_number)
fisheries$asfis_species_number <- as.factor(fisheries$asfis_species_number)
fisheries$fao_major_fishing_area <- as.factor(fisheries$fao_major_fishing_area)
```

We need to deal with the years because they are being treated as characters and start with an X. We also have the problem that the column names that are years actually represent data. We haven't discussed tidy data yet, so here is some help. You should run this ugly chunk to transform the data for the rest of the homework. It will only work if you have used janitor to rename the variables in question 2!

```r
fisheries_tidy <- fisheries %>% 
  pivot_longer(-c(country,common_name,isscaap_group_number,isscaap_taxonomic_group,asfis_species_number,asfis_species_name,fao_major_fishing_area,measure),
               names_to = "year",
               values_to = "catch",
               values_drop_na = TRUE) %>% 
  mutate(year= as.numeric(str_replace(year, 'x', ''))) %>% 
  mutate(catch= str_replace(catch, c(' F'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('...'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('-'), replacement = '')) %>% 
  mutate(catch= str_replace(catch, c('0 0'), replacement = ''))

fisheries_tidy$catch <- as.numeric(fisheries_tidy$catch)
fisheries_tidy
```

```
## # A tibble: 376,771 x 10
##    country common_name isscaap_group_n~ isscaap_taxonom~ asfis_species_n~
##    <fct>   <chr>       <fct>            <chr>            <fct>           
##  1 Albania Angelshark~ 38               Sharks, rays, c~ 10903XXXXX      
##  2 Albania Angelshark~ 38               Sharks, rays, c~ 10903XXXXX      
##  3 Albania Angelshark~ 38               Sharks, rays, c~ 10903XXXXX      
##  4 Albania Angelshark~ 38               Sharks, rays, c~ 10903XXXXX      
##  5 Albania Angelshark~ 38               Sharks, rays, c~ 10903XXXXX      
##  6 Albania Angelshark~ 38               Sharks, rays, c~ 10903XXXXX      
##  7 Albania Angelshark~ 38               Sharks, rays, c~ 10903XXXXX      
##  8 Albania Angelshark~ 38               Sharks, rays, c~ 10903XXXXX      
##  9 Albania Angelshark~ 38               Sharks, rays, c~ 10903XXXXX      
## 10 Albania Angelshark~ 38               Sharks, rays, c~ 10903XXXXX      
## # ... with 376,761 more rows, and 5 more variables: asfis_species_name <chr>,
## #   fao_major_fishing_area <fct>, measure <chr>, year <dbl>, catch <dbl>
```

3. How many countries are represented in the data? Provide a count and list their names.

```r
fisheries_tidy %>% 
  summarize(distinct.country=n_distinct(country))
```

```
## # A tibble: 1 x 1
##   distinct.country
##              <int>
## 1              203
```


```r
fisheries_tidy %>% 
  count(country,sort=T)
```

```
## # A tibble: 203 x 2
##    country                      n
##    <fct>                    <int>
##  1 United States of America 18080
##  2 Spain                    17482
##  3 Japan                    15429
##  4 Portugal                 11570
##  5 Korea, Republic of       10824
##  6 France                   10639
##  7 Taiwan Province of China  9927
##  8 Indonesia                 9274
##  9 Australia                 8183
## 10 Un. Sov. Soc. Rep.        7084
## # ... with 193 more rows
```
203 countries
4. Refocus the data only to include only: country, isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch.

```r
fisheries_tidy %>% 
  select(country, isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch)
```

```
## # A tibble: 376,771 x 6
##    country isscaap_taxonomic_g~ asfis_species_na~ asfis_species_num~  year catch
##    <fct>   <chr>                <chr>             <fct>              <dbl> <dbl>
##  1 Albania Sharks, rays, chima~ Squatinidae       10903XXXXX          1995    NA
##  2 Albania Sharks, rays, chima~ Squatinidae       10903XXXXX          1996    53
##  3 Albania Sharks, rays, chima~ Squatinidae       10903XXXXX          1997    20
##  4 Albania Sharks, rays, chima~ Squatinidae       10903XXXXX          1998    31
##  5 Albania Sharks, rays, chima~ Squatinidae       10903XXXXX          1999    30
##  6 Albania Sharks, rays, chima~ Squatinidae       10903XXXXX          2000    30
##  7 Albania Sharks, rays, chima~ Squatinidae       10903XXXXX          2001    16
##  8 Albania Sharks, rays, chima~ Squatinidae       10903XXXXX          2002    79
##  9 Albania Sharks, rays, chima~ Squatinidae       10903XXXXX          2003     1
## 10 Albania Sharks, rays, chima~ Squatinidae       10903XXXXX          2004     4
## # ... with 376,761 more rows
```

5. Based on the asfis_species_number, how many distinct fish species were caught as part of these data?

```r
fisheries_tidy %>% 
  summarize(distinct.asfis_species_number=n_distinct(asfis_species_number))
```

```
## # A tibble: 1 x 1
##   distinct.asfis_species_number
##                           <int>
## 1                          1551
```
1551 different species
6. Which country had the largest overall catch in the year 2000?

```r
fisheries_tidy %>%
  filter(year=="2000") %>% 
  group_by(country) %>% 
  summarize(sumcatch=sum(catch,na.rm=T),
            totaln=n(),.groups= 'keep') %>% 
  arrange(desc(sumcatch))
```

```
## # A tibble: 193 x 3
## # Groups:   country [193]
##    country                  sumcatch totaln
##    <fct>                       <dbl>  <int>
##  1 China                       25899     93
##  2 Russian Federation          12181    192
##  3 United States of America    11762    438
##  4 Japan                        8510    241
##  5 Indonesia                    8341    140
##  6 Peru                         7443     54
##  7 Chile                        6906     91
##  8 India                        6351     98
##  9 Thailand                     6243    127
## 10 Korea, Republic of           6124    265
## # ... with 183 more rows
```
China had the largest overall catch in 2000
7. Which country caught the most sardines (_Sardina pilchardus_) between the years 1990-2000?

```r
fisheries_tidy %>% 
  filter(year>=1990, year<=2000,asfis_species_name=="Sardina pilchardus") %>%
  group_by(country) %>% 
  summarize(catchsum=sum(catch,na.rm=T), .groups= 'keep') %>% 
  arrange(desc(catchsum))
```

```
## # A tibble: 37 x 2
## # Groups:   country [37]
##    country               catchsum
##    <fct>                    <dbl>
##  1 Morocco                   7470
##  2 Spain                     3507
##  3 Russian Federation        1639
##  4 Ukraine                   1030
##  5 France                     966
##  6 Portugal                   818
##  7 Greece                     528
##  8 Italy                      507
##  9 Serbia and Montenegro      478
## 10 Denmark                    477
## # ... with 27 more rows
```
Morocco caught the most sardines between 1990 to 2000

<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 20px;}
</style>
<div class = "blue">

8. Which five countries caught the most cephalopods between 2008-2012?

```r
fisheries_tidy %>% 
  filter(year>=2008,year<=2012, asfis_species_name=="Cephalopoda") %>% 
  group_by(country) %>% 
  summarize(sumcatch=sum(catch,na.rm=T),.groups= 'keep') %>% 
  arrange(desc(sumcatch))
```

```
## # A tibble: 16 x 2
## # Groups:   country [16]
##    country                  sumcatch
##    <fct>                       <dbl>
##  1 India                         570
##  2 China                         257
##  3 Spain                         198
##  4 Algeria                       162
##  5 France                        101
##  6 Mauritania                     90
##  7 TimorLeste                     76
##  8 Italy                          66
##  9 Mozambique                     16
## 10 Cambodia                       15
## 11 Taiwan Province of China       13
## 12 Madagascar                     11
## 13 Croatia                         7
## 14 Israel                          0
## 15 Somalia                         0
## 16 Viet Nam                        0
```
India, China, Spain, Algeria and France

</div>

9. Which species had the highest catch total between 2008-2012? (hint: Osteichthyes is not a species)

```r
fisheries_tidy %>%
  filter(year>=2008, year<=2012) %>%
  group_by(asfis_species_name) %>%
  summarize(species_catch_count=sum(catch,na.rm=T), .groups= 'keep') %>% 
  arrange(desc(species_catch_count))
```

```
## # A tibble: 1,472 x 2
## # Groups:   asfis_species_name [1,472]
##    asfis_species_name    species_catch_count
##    <chr>                               <dbl>
##  1 Osteichthyes                       107808
##  2 Theragra chalcogramma               41075
##  3 Engraulis ringens                   35523
##  4 Katsuwonus pelamis                  32153
##  5 Trichiurus lepturus                 30400
##  6 Clupea harengus                     28527
##  7 Thunnus albacares                   20119
##  8 Scomber japonicus                   14723
##  9 Gadus morhua                        13253
## 10 Thunnus alalunga                    12019
## # ... with 1,462 more rows
```
Theragra chalcogramma had the highest catch total between 2008 to 2012
10. Use the data to do at least one analysis of your choice.

```r
fisheries_tidy %>% 
  filter(country=="China") %>% 
  group_by(country) %>% 
  summarize(total.catch=sum(catch,na.rm=T),
            std.catch=sd(catch,na.rm=T),
            mean.catch=mean(catch,na.rm=T), .groups= 'keep')
```

```
## # A tibble: 1 x 4
## # Groups:   country [1]
##   country total.catch std.catch mean.catch
##   <fct>         <dbl>     <dbl>      <dbl>
## 1 China        735938     1046.       321.
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
