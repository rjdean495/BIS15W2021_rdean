---
title: "Lab 5 Homework"
author: "Richard J. Dean"
date: "2021-01-20"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Load the superhero data
These are data taken from comic books and assembled by fans. The include a good mix of categorical and continuous data.  Data taken from: https://www.kaggle.com/claudiodavi/superhero-set  

Check out the way I am loading these data. If I know there are NAs, I can take care of them at the beginning. But, we should do this very cautiously. At times it is better to keep the original columns and data intact.  

```r
superhero_info <- readr::read_csv("data/heroes_information.csv", na = c("", "-99", "-"))
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   name = col_character(),
##   Gender = col_character(),
##   `Eye color` = col_character(),
##   Race = col_character(),
##   `Hair color` = col_character(),
##   Height = col_double(),
##   Publisher = col_character(),
##   `Skin color` = col_character(),
##   Alignment = col_character(),
##   Weight = col_double()
## )
```

```r
superhero_powers <- readr::read_csv("data/super_hero_powers.csv", na = c("", "-99", "-"))
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   .default = col_logical(),
##   hero_names = col_character()
## )
## ℹ Use `spec()` for the full column specifications.
```

## Data tidy
1. Some of the names used in the `superhero_info` data are problematic so you should rename them here.  

```r
names(superhero_info)
```

```
##  [1] "name"       "Gender"     "Eye color"  "Race"       "Hair color"
##  [6] "Height"     "Publisher"  "Skin color" "Alignment"  "Weight"
```

```r
superhero_info<-rename(superhero_info, gender="Gender",eye.color="Eye color", race="Race", hair.color="Hair color", height="Height", publisher="Publisher", skin.color="Skin color", alignment="Alignment", weight="Weight" )
```

Yikes! `superhero_powers` has a lot of variables that are poorly named. We need some R superpowers...

```r
head(superhero_powers)
```

```
## # A tibble: 6 x 168
##   hero_names Agility `Accelerated He… `Lantern Power … `Dimensional Aw…
##   <chr>      <lgl>   <lgl>            <lgl>            <lgl>           
## 1 3-D Man    TRUE    FALSE            FALSE            FALSE           
## 2 A-Bomb     FALSE   TRUE             FALSE            FALSE           
## 3 Abe Sapien TRUE    TRUE             FALSE            FALSE           
## 4 Abin Sur   FALSE   FALSE            TRUE             FALSE           
## 5 Abominati… FALSE   TRUE             FALSE            FALSE           
## 6 Abraxas    FALSE   FALSE            FALSE            TRUE            
## # … with 163 more variables: `Cold Resistance` <lgl>, Durability <lgl>,
## #   Stealth <lgl>, `Energy Absorption` <lgl>, Flight <lgl>, `Danger
## #   Sense` <lgl>, `Underwater breathing` <lgl>, Marksmanship <lgl>, `Weapons
## #   Master` <lgl>, `Power Augmentation` <lgl>, `Animal Attributes` <lgl>,
## #   Longevity <lgl>, Intelligence <lgl>, `Super Strength` <lgl>,
## #   Cryokinesis <lgl>, Telepathy <lgl>, `Energy Armor` <lgl>, `Energy
## #   Blasts` <lgl>, Duplication <lgl>, `Size Changing` <lgl>, `Density
## #   Control` <lgl>, Stamina <lgl>, `Astral Travel` <lgl>, `Audio
## #   Control` <lgl>, Dexterity <lgl>, Omnitrix <lgl>, `Super Speed` <lgl>,
## #   Possession <lgl>, `Animal Oriented Powers` <lgl>, `Weapon-based
## #   Powers` <lgl>, Electrokinesis <lgl>, `Darkforce Manipulation` <lgl>, `Death
## #   Touch` <lgl>, Teleportation <lgl>, `Enhanced Senses` <lgl>,
## #   Telekinesis <lgl>, `Energy Beams` <lgl>, Magic <lgl>, Hyperkinesis <lgl>,
## #   Jump <lgl>, Clairvoyance <lgl>, `Dimensional Travel` <lgl>, `Power
## #   Sense` <lgl>, Shapeshifting <lgl>, `Peak Human Condition` <lgl>,
## #   Immortality <lgl>, Camouflage <lgl>, `Element Control` <lgl>,
## #   Phasing <lgl>, `Astral Projection` <lgl>, `Electrical Transport` <lgl>,
## #   `Fire Control` <lgl>, Projection <lgl>, Summoning <lgl>, `Enhanced
## #   Memory` <lgl>, Reflexes <lgl>, Invulnerability <lgl>, `Energy
## #   Constructs` <lgl>, `Force Fields` <lgl>, `Self-Sustenance` <lgl>,
## #   `Anti-Gravity` <lgl>, Empathy <lgl>, `Power Nullifier` <lgl>, `Radiation
## #   Control` <lgl>, `Psionic Powers` <lgl>, Elasticity <lgl>, `Substance
## #   Secretion` <lgl>, `Elemental Transmogrification` <lgl>,
## #   `Technopath/Cyberpath` <lgl>, `Photographic Reflexes` <lgl>, `Seismic
## #   Power` <lgl>, Animation <lgl>, Precognition <lgl>, `Mind Control` <lgl>,
## #   `Fire Resistance` <lgl>, `Power Absorption` <lgl>, `Enhanced
## #   Hearing` <lgl>, `Nova Force` <lgl>, Insanity <lgl>, Hypnokinesis <lgl>,
## #   `Animal Control` <lgl>, `Natural Armor` <lgl>, Intangibility <lgl>,
## #   `Enhanced Sight` <lgl>, `Molecular Manipulation` <lgl>, `Heat
## #   Generation` <lgl>, Adaptation <lgl>, Gliding <lgl>, `Power Suit` <lgl>,
## #   `Mind Blast` <lgl>, `Probability Manipulation` <lgl>, `Gravity
## #   Control` <lgl>, Regeneration <lgl>, `Light Control` <lgl>,
## #   Echolocation <lgl>, Levitation <lgl>, `Toxin and Disease Control` <lgl>,
## #   Banish <lgl>, `Energy Manipulation` <lgl>, `Heat Resistance` <lgl>, …
```

## `janitor`
The [janitor](https://garthtarr.github.io/meatR/janitor.html) package is your friend. Make sure to install it and then load the library.  

```r
library("janitor")
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

The `clean_names` function takes care of everything in one line! Now that's a superpower!

```r
superhero_powers <- janitor::clean_names(superhero_powers)
```

## `tabyl`
The `janitor` package has many awesome functions that we will explore. Here is its version of `table` which not only produces counts but also percentages. Very handy! Let's use it to explore the proportion of good guys and bad guys in the `superhero_info` data.  

```r
tabyl(superhero_info, alignment)
```

```
##  alignment   n     percent valid_percent
##        bad 207 0.282016349    0.28473177
##       good 496 0.675749319    0.68225585
##    neutral  24 0.032697548    0.03301238
##       <NA>   7 0.009536785            NA
```

2. Notice that we have some neutral superheros! Who are they?

```r
superhero_info %>% 
  select(name,alignment) %>% 
  filter(alignment=="neutral")
```

```
## # A tibble: 24 x 2
##    name         alignment
##    <chr>        <chr>    
##  1 Bizarro      neutral  
##  2 Black Flash  neutral  
##  3 Captain Cold neutral  
##  4 Copycat      neutral  
##  5 Deadpool     neutral  
##  6 Deathstroke  neutral  
##  7 Etrigan      neutral  
##  8 Galactus     neutral  
##  9 Gladiator    neutral  
## 10 Indigo       neutral  
## # … with 14 more rows
```

## `superhero_info`
3. Let's say we are only interested in the variables name, alignment, and "race". How would you isolate these variables from `superhero_info`?

```r
superhero_info %>% 
  select(name,alignment,race)
```

```
## # A tibble: 734 x 3
##    name          alignment race             
##    <chr>         <chr>     <chr>            
##  1 A-Bomb        good      Human            
##  2 Abe Sapien    good      Icthyo Sapien    
##  3 Abin Sur      good      Ungaran          
##  4 Abomination   bad       Human / Radiation
##  5 Abraxas       bad       Cosmic Entity    
##  6 Absorbing Man bad       Human            
##  7 Adam Monroe   good      <NA>             
##  8 Adam Strange  good      Human            
##  9 Agent 13      good      <NA>             
## 10 Agent Bob     good      Human            
## # … with 724 more rows
```

## Not Human
4. List all of the superheros that are not human.

```r
superhero_info %>% 
  select(name,race) %>% 
  filter(race!="Human")
```

```
## # A tibble: 222 x 2
##    name         race             
##    <chr>        <chr>            
##  1 Abe Sapien   Icthyo Sapien    
##  2 Abin Sur     Ungaran          
##  3 Abomination  Human / Radiation
##  4 Abraxas      Cosmic Entity    
##  5 Ajax         Cyborg           
##  6 Alien        Xenomorph XX121  
##  7 Amazo        Android          
##  8 Angel        Vampire          
##  9 Angel Dust   Mutant           
## 10 Anti-Monitor God / Eternal    
## # … with 212 more rows
```

## Good and Evil
5. Let's make two different data frames, one focused on the "good guys" and another focused on the "bad guys".

```r
good<-superhero_info %>% 
  filter(alignment=="good")
bad<-superhero_info %>% 
  filter(alignment=="bad")
```

6. For the good guys, use the `tabyl` function to summarize their "race".

```r
tabyl(good,race)
```

```
##               race   n     percent valid_percent
##              Alien   3 0.006048387   0.010752688
##              Alpha   5 0.010080645   0.017921147
##             Amazon   2 0.004032258   0.007168459
##            Android   4 0.008064516   0.014336918
##             Animal   2 0.004032258   0.007168459
##          Asgardian   3 0.006048387   0.010752688
##          Atlantean   4 0.008064516   0.014336918
##         Bolovaxian   1 0.002016129   0.003584229
##              Clone   1 0.002016129   0.003584229
##             Cyborg   3 0.006048387   0.010752688
##           Demi-God   2 0.004032258   0.007168459
##              Demon   3 0.006048387   0.010752688
##            Eternal   1 0.002016129   0.003584229
##     Flora Colossus   1 0.002016129   0.003584229
##        Frost Giant   1 0.002016129   0.003584229
##      God / Eternal   6 0.012096774   0.021505376
##             Gungan   1 0.002016129   0.003584229
##              Human 148 0.298387097   0.530465950
##    Human / Altered   2 0.004032258   0.007168459
##     Human / Cosmic   2 0.004032258   0.007168459
##  Human / Radiation   8 0.016129032   0.028673835
##         Human-Kree   2 0.004032258   0.007168459
##      Human-Spartoi   1 0.002016129   0.003584229
##       Human-Vulcan   1 0.002016129   0.003584229
##    Human-Vuldarian   1 0.002016129   0.003584229
##      Icthyo Sapien   1 0.002016129   0.003584229
##            Inhuman   4 0.008064516   0.014336918
##    Kakarantharaian   1 0.002016129   0.003584229
##         Kryptonian   4 0.008064516   0.014336918
##            Martian   1 0.002016129   0.003584229
##          Metahuman   1 0.002016129   0.003584229
##             Mutant  46 0.092741935   0.164874552
##     Mutant / Clone   1 0.002016129   0.003584229
##             Planet   1 0.002016129   0.003584229
##             Saiyan   1 0.002016129   0.003584229
##           Symbiote   3 0.006048387   0.010752688
##           Talokite   1 0.002016129   0.003584229
##         Tamaranean   1 0.002016129   0.003584229
##            Ungaran   1 0.002016129   0.003584229
##            Vampire   2 0.004032258   0.007168459
##     Yoda's species   1 0.002016129   0.003584229
##      Zen-Whoberian   1 0.002016129   0.003584229
##               <NA> 217 0.437500000            NA
```

7. Among the good guys, Who are the Asgardians?

```r
good %>% 
  filter(race=="Asgardian")
```

```
## # A tibble: 3 x 10
##   name  gender eye.color race  hair.color height publisher skin.color alignment
##   <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Sif   Female blue      Asga… Black         188 Marvel C… <NA>       good     
## 2 Thor  Male   blue      Asga… Blond         198 Marvel C… <NA>       good     
## 3 Thor… Female blue      Asga… Blond         175 Marvel C… <NA>       good     
## # … with 1 more variable: weight <dbl>
```

8. Among the bad guys, who are the male humans over 200 inches in height?

```r
bad %>% 
  filter(gender=="Male"&height>200&race=="Human")
```

```
## # A tibble: 5 x 10
##   name  gender eye.color race  hair.color height publisher skin.color alignment
##   <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Bane  Male   <NA>      Human <NA>          203 DC Comics <NA>       bad      
## 2 Doct… Male   brown     Human Brown         201 Marvel C… <NA>       bad      
## 3 King… Male   blue      Human No Hair       201 Marvel C… <NA>       bad      
## 4 Liza… Male   red       Human No Hair       203 Marvel C… <NA>       bad      
## 5 Scor… Male   brown     Human Brown         211 Marvel C… <NA>       bad      
## # … with 1 more variable: weight <dbl>
```

9. OK, so are there more good guys or bad guys that are bald (personal interest)?

```r
good %>% 
  filter(hair.color=="No Hair") %>% 
  select(name,hair.color)
```

```
## # A tibble: 37 x 2
##    name            hair.color
##    <chr>           <chr>     
##  1 A-Bomb          No Hair   
##  2 Abe Sapien      No Hair   
##  3 Abin Sur        No Hair   
##  4 Beta Ray Bill   No Hair   
##  5 Bishop          No Hair   
##  6 Black Lightning No Hair   
##  7 Blaquesmith     No Hair   
##  8 Bloodhawk       No Hair   
##  9 Crimson Dynamo  No Hair   
## 10 Donatello       No Hair   
## # … with 27 more rows
```

```r
bad %>% 
  filter(hair.color=="No Hair") %>% 
  select(name,hair.color)
```

```
## # A tibble: 35 x 2
##    name          hair.color
##    <chr>         <chr>     
##  1 Abomination   No Hair   
##  2 Absorbing Man No Hair   
##  3 Alien         No Hair   
##  4 Annihilus     No Hair   
##  5 Anti-Monitor  No Hair   
##  6 Black Manta   No Hair   
##  7 Bloodwraith   No Hair   
##  8 Brainiac      No Hair   
##  9 Darkseid      No Hair   
## 10 Darth Vader   No Hair   
## # … with 25 more rows
```
There are more good guys who are bald
10. Let's explore who the really "big" superheros are. In the `superhero_info` data, which have a height over 200 or weight over 300?

```r
superhero_info %>% 
  filter(height>300 | weight>450) %>% 
  select(name, height, weight)
```

```
## # A tibble: 14 x 3
##    name          height weight
##    <chr>          <dbl>  <dbl>
##  1 Bloodaxe       218      495
##  2 Darkseid       267      817
##  3 Fin Fang Foom  975       18
##  4 Galactus       876       16
##  5 Giganta         62.5    630
##  6 Groot          701        4
##  7 Hulk           244      630
##  8 Juggernaut     287      855
##  9 MODOK          366      338
## 10 Onslaught      305      405
## 11 Red Hulk       213      630
## 12 Sasquatch      305      900
## 13 Wolfsbane      366      473
## 14 Ymir           305.      NA
```

11. Just to be clear on the `|` operator,  have a look at the superheros over 300 in height...

```r
superhero_info %>% 
  filter(height>300)
```

```
## # A tibble: 8 x 10
##   name  gender eye.color race  hair.color height publisher skin.color alignment
##   <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Fin … Male   red       Kaka… No Hair      975  Marvel C… green      good     
## 2 Gala… Male   black     Cosm… Black        876  Marvel C… <NA>       neutral  
## 3 Groot Male   yellow    Flor… <NA>         701  Marvel C… <NA>       good     
## 4 MODOK Male   white     Cybo… Brownn       366  Marvel C… <NA>       bad      
## 5 Onsl… Male   red       Muta… No Hair      305  Marvel C… <NA>       bad      
## 6 Sasq… Male   red       <NA>  Orange       305  Marvel C… <NA>       good     
## 7 Wolf… Female green     <NA>  Auburn       366  Marvel C… <NA>       good     
## 8 Ymir  Male   white     Fros… No Hair      305. Marvel C… white      good     
## # … with 1 more variable: weight <dbl>
```

12. ...and the superheros over 450 in weight. Bonus question! Why do we not have 16 rows in question #10?

```r
superhero_info %>% 
  filter(weight>450)
```

```
## # A tibble: 8 x 10
##   name  gender eye.color race  hair.color height publisher skin.color alignment
##   <chr> <chr>  <chr>     <chr> <chr>       <dbl> <chr>     <chr>      <chr>    
## 1 Bloo… Female blue      Human Brown       218   Marvel C… <NA>       bad      
## 2 Dark… Male   red       New … No Hair     267   DC Comics grey       bad      
## 3 Giga… Female green     <NA>  Red          62.5 DC Comics <NA>       bad      
## 4 Hulk  Male   green     Huma… Green       244   Marvel C… green      good     
## 5 Jugg… Male   blue      Human Red         287   Marvel C… <NA>       neutral  
## 6 Red … Male   yellow    Huma… Black       213   Marvel C… red        neutral  
## 7 Sasq… Male   red       <NA>  Orange      305   Marvel C… <NA>       good     
## 8 Wolf… Female green     <NA>  Auburn      366   Marvel C… <NA>       good     
## # … with 1 more variable: weight <dbl>
```

## Height to Weight Ratio
13. It's easy to be strong when you are heavy and tall, but who is heavy and short? Which superheros have the highest height to weight ratio?

```r
superhero_info %>% 
  mutate(HWR=height/weight) %>% 
  select(name, height, weight, HWR) %>% 
  arrange(HWR)
```

```
## # A tibble: 734 x 4
##    name        height weight    HWR
##    <chr>        <dbl>  <dbl>  <dbl>
##  1 Giganta       62.5    630 0.0992
##  2 Utgard-Loki   15.2     58 0.262 
##  3 Darkseid     267      817 0.327 
##  4 Juggernaut   287      855 0.336 
##  5 Red Hulk     213      630 0.338 
##  6 Sasquatch    305      900 0.339 
##  7 Hulk         244      630 0.387 
##  8 Bloodaxe     218      495 0.440 
##  9 Thanos       201      443 0.454 
## 10 A-Bomb       203      441 0.460 
## # … with 724 more rows
```

## `superhero_powers`
Have a quick look at the `superhero_powers` data frame.  

```r
head(superhero_powers)
```

```
## # A tibble: 6 x 168
##   hero_names agility accelerated_hea… lantern_power_r… dimensional_awa…
##   <chr>      <lgl>   <lgl>            <lgl>            <lgl>           
## 1 3-D Man    TRUE    FALSE            FALSE            FALSE           
## 2 A-Bomb     FALSE   TRUE             FALSE            FALSE           
## 3 Abe Sapien TRUE    TRUE             FALSE            FALSE           
## 4 Abin Sur   FALSE   FALSE            TRUE             FALSE           
## 5 Abominati… FALSE   TRUE             FALSE            FALSE           
## 6 Abraxas    FALSE   FALSE            FALSE            TRUE            
## # … with 163 more variables: cold_resistance <lgl>, durability <lgl>,
## #   stealth <lgl>, energy_absorption <lgl>, flight <lgl>, danger_sense <lgl>,
## #   underwater_breathing <lgl>, marksmanship <lgl>, weapons_master <lgl>,
## #   power_augmentation <lgl>, animal_attributes <lgl>, longevity <lgl>,
## #   intelligence <lgl>, super_strength <lgl>, cryokinesis <lgl>,
## #   telepathy <lgl>, energy_armor <lgl>, energy_blasts <lgl>,
## #   duplication <lgl>, size_changing <lgl>, density_control <lgl>,
## #   stamina <lgl>, astral_travel <lgl>, audio_control <lgl>, dexterity <lgl>,
## #   omnitrix <lgl>, super_speed <lgl>, possession <lgl>,
## #   animal_oriented_powers <lgl>, weapon_based_powers <lgl>,
## #   electrokinesis <lgl>, darkforce_manipulation <lgl>, death_touch <lgl>,
## #   teleportation <lgl>, enhanced_senses <lgl>, telekinesis <lgl>,
## #   energy_beams <lgl>, magic <lgl>, hyperkinesis <lgl>, jump <lgl>,
## #   clairvoyance <lgl>, dimensional_travel <lgl>, power_sense <lgl>,
## #   shapeshifting <lgl>, peak_human_condition <lgl>, immortality <lgl>,
## #   camouflage <lgl>, element_control <lgl>, phasing <lgl>,
## #   astral_projection <lgl>, electrical_transport <lgl>, fire_control <lgl>,
## #   projection <lgl>, summoning <lgl>, enhanced_memory <lgl>, reflexes <lgl>,
## #   invulnerability <lgl>, energy_constructs <lgl>, force_fields <lgl>,
## #   self_sustenance <lgl>, anti_gravity <lgl>, empathy <lgl>,
## #   power_nullifier <lgl>, radiation_control <lgl>, psionic_powers <lgl>,
## #   elasticity <lgl>, substance_secretion <lgl>,
## #   elemental_transmogrification <lgl>, technopath_cyberpath <lgl>,
## #   photographic_reflexes <lgl>, seismic_power <lgl>, animation <lgl>,
## #   precognition <lgl>, mind_control <lgl>, fire_resistance <lgl>,
## #   power_absorption <lgl>, enhanced_hearing <lgl>, nova_force <lgl>,
## #   insanity <lgl>, hypnokinesis <lgl>, animal_control <lgl>,
## #   natural_armor <lgl>, intangibility <lgl>, enhanced_sight <lgl>,
## #   molecular_manipulation <lgl>, heat_generation <lgl>, adaptation <lgl>,
## #   gliding <lgl>, power_suit <lgl>, mind_blast <lgl>,
## #   probability_manipulation <lgl>, gravity_control <lgl>, regeneration <lgl>,
## #   light_control <lgl>, echolocation <lgl>, levitation <lgl>,
## #   toxin_and_disease_control <lgl>, banish <lgl>, energy_manipulation <lgl>,
## #   heat_resistance <lgl>, …
```

14. How many superheros have a combination of accelerated healing, durability, and super strength?

```r
superhero_powers %>% 
  filter(accelerated_healing=="TRUE"&durability=="TRUE"&super_strength=="TRUE") %>% 
  select(hero_names,accelerated_healing,durability,super_strength)
```

```
## # A tibble: 97 x 4
##    hero_names   accelerated_healing durability super_strength
##    <chr>        <lgl>               <lgl>      <lgl>         
##  1 A-Bomb       TRUE                TRUE       TRUE          
##  2 Abe Sapien   TRUE                TRUE       TRUE          
##  3 Angel        TRUE                TRUE       TRUE          
##  4 Anti-Monitor TRUE                TRUE       TRUE          
##  5 Anti-Venom   TRUE                TRUE       TRUE          
##  6 Aquaman      TRUE                TRUE       TRUE          
##  7 Arachne      TRUE                TRUE       TRUE          
##  8 Archangel    TRUE                TRUE       TRUE          
##  9 Ardina       TRUE                TRUE       TRUE          
## 10 Ares         TRUE                TRUE       TRUE          
## # … with 87 more rows
```

## `kinesis`
15. We are only interested in the superheros that do some kind of "kinesis". How would we isolate them from the `superhero_powers` data?

```r
superhero_powers %>% 
  select(hero_names,contains("kinesis"))
```

```
## # A tibble: 667 x 10
##    hero_names cryokinesis electrokinesis telekinesis hyperkinesis hypnokinesis
##    <chr>      <lgl>       <lgl>          <lgl>       <lgl>        <lgl>       
##  1 3-D Man    FALSE       FALSE          FALSE       FALSE        FALSE       
##  2 A-Bomb     FALSE       FALSE          FALSE       FALSE        FALSE       
##  3 Abe Sapien FALSE       FALSE          FALSE       FALSE        FALSE       
##  4 Abin Sur   FALSE       FALSE          FALSE       FALSE        FALSE       
##  5 Abominati… FALSE       FALSE          FALSE       FALSE        FALSE       
##  6 Abraxas    FALSE       FALSE          FALSE       FALSE        FALSE       
##  7 Absorbing… FALSE       FALSE          FALSE       FALSE        FALSE       
##  8 Adam Monr… FALSE       FALSE          FALSE       FALSE        FALSE       
##  9 Adam Stra… FALSE       FALSE          FALSE       FALSE        FALSE       
## 10 Agent Bob  FALSE       FALSE          FALSE       FALSE        FALSE       
## # … with 657 more rows, and 4 more variables: thirstokinesis <lgl>,
## #   biokinesis <lgl>, terrakinesis <lgl>, vitakinesis <lgl>
```

16. Pick your favorite superhero and let's see their powers!

```r
superhero_powers %>% 
  filter(hero_names=="Aquababy") %>% 
  select_if(all_vars(.=="TRUE"))
```

```
## Warning: The `.predicate` argument of `select_if()` can't contain quosures. as of dplyr 0.8.3.
## Please use a one-sided formula, a function, or a function name.
## This warning is displayed once every 8 hours.
## Call `lifecycle::last_warnings()` to see where this warning was generated.
```

```
## # A tibble: 1 x 3
##   underwater_breathing super_strength water_control
##   <lgl>                <lgl>          <lgl>        
## 1 TRUE                 TRUE           TRUE
```
