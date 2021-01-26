---
title: "Lab 5 Homework"
author: "Please Add Your Name Here"
date: "2021-01-25"
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
## -- Column specification --------------------------------------------------------
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
## -- Column specification --------------------------------------------------------
## cols(
##   .default = col_logical(),
##   hero_names = col_character()
## )
## i Use `spec()` for the full column specifications.
```

## Data tidy
1. Some of the names used in the `superhero_info` data are problematic so you should rename them here.  \


```r
names(superhero_info)
```

```
##  [1] "name"       "Gender"     "Eye color"  "Race"       "Hair color"
##  [6] "Height"     "Publisher"  "Skin color" "Alignment"  "Weight"
```


```r
superhero_info <- rename(superhero_info, name=name, gender=Gender, eye_color='Eye color', race=Race, hair_color='Hair color', height=Height, publisher=Publisher, skin_color='Skin color', alignment=Alignment, weight=Weight)
```




```r
superhero_info <- superhero_info %>%
  mutate_all(tolower)
superhero_info
```

```
## # A tibble: 734 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>      <chr>  <chr>     <chr>      <chr>    
##  1 a-bo~ male   yellow    human no hair    203    marvel c~ <NA>       good     
##  2 abe ~ male   blue      icth~ no hair    191    dark hor~ blue       good     
##  3 abin~ male   blue      unga~ no hair    185    dc comics red        good     
##  4 abom~ male   green     huma~ no hair    203    marvel c~ <NA>       bad      
##  5 abra~ male   blue      cosm~ black      <NA>   marvel c~ <NA>       bad      
##  6 abso~ male   blue      human no hair    193    marvel c~ <NA>       bad      
##  7 adam~ male   blue      <NA>  blond      <NA>   nbc - he~ <NA>       good     
##  8 adam~ male   blue      human blond      185    dc comics <NA>       good     
##  9 agen~ female blue      <NA>  blond      173    marvel c~ <NA>       good     
## 10 agen~ male   brown     human brown      178    marvel c~ <NA>       good     
## # ... with 724 more rows, and 1 more variable: weight <chr>
```





```r
names(superhero_info)
```

```
##  [1] "name"       "gender"     "eye_color"  "race"       "hair_color"
##  [6] "height"     "publisher"  "skin_color" "alignment"  "weight"
```

Yikes! `superhero_powers` has a lot of variables that are poorly named. We need some R superpowers...

```r
head(superhero_powers)
```

```
## # A tibble: 6 x 168
##   hero_names Agility `Accelerated He~ `Lantern Power ~ `Dimensional Aw~
##   <chr>      <lgl>   <lgl>            <lgl>            <lgl>           
## 1 3-D Man    TRUE    FALSE            FALSE            FALSE           
## 2 A-Bomb     FALSE   TRUE             FALSE            FALSE           
## 3 Abe Sapien TRUE    TRUE             FALSE            FALSE           
## 4 Abin Sur   FALSE   FALSE            TRUE             FALSE           
## 5 Abominati~ FALSE   TRUE             FALSE            FALSE           
## 6 Abraxas    FALSE   FALSE            FALSE            TRUE            
## # ... with 163 more variables: `Cold Resistance` <lgl>, Durability <lgl>,
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
## #   Banish <lgl>, `Energy Manipulation` <lgl>, `Heat Resistance` <lgl>, ...
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

```r
head(superhero_powers)
```

```
## # A tibble: 6 x 168
##   hero_names agility accelerated_hea~ lantern_power_r~ dimensional_awa~
##   <chr>      <lgl>   <lgl>            <lgl>            <lgl>           
## 1 3-D Man    TRUE    FALSE            FALSE            FALSE           
## 2 A-Bomb     FALSE   TRUE             FALSE            FALSE           
## 3 Abe Sapien TRUE    TRUE             FALSE            FALSE           
## 4 Abin Sur   FALSE   FALSE            TRUE             FALSE           
## 5 Abominati~ FALSE   TRUE             FALSE            FALSE           
## 6 Abraxas    FALSE   FALSE            FALSE            TRUE            
## # ... with 163 more variables: cold_resistance <lgl>, durability <lgl>,
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
## #   heat_resistance <lgl>, ...
```

## `tabyl`
The `janitor` package has many awesome functions that we will explore. Here is its version of `table` which not only produces counts but also percentages. Very handy! Let's use it to explore the proportion of good guys and bad guys in the `superhero_info` data.  

```r
superhero_info %>%
  tabyl( alignment)
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
  filter(alignment == "neutral")
```

```
## # A tibble: 24 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>      <chr>  <chr>     <chr>      <chr>    
##  1 biza~ male   black     biza~ black      191    dc comics white      neutral  
##  2 blac~ male   <NA>      god ~ <NA>       <NA>   dc comics <NA>       neutral  
##  3 capt~ male   brown     human brown      <NA>   dc comics <NA>       neutral  
##  4 copy~ female red       muta~ white      183    marvel c~ blue       neutral  
##  5 dead~ male   brown     muta~ no hair    188    marvel c~ <NA>       neutral  
##  6 deat~ male   blue      human white      193    dc comics <NA>       neutral  
##  7 etri~ male   red       demon no hair    193    dc comics yellow     neutral  
##  8 gala~ male   black     cosm~ black      876    marvel c~ <NA>       neutral  
##  9 glad~ male   blue      stro~ blue       198    marvel c~ purple     neutral  
## 10 indi~ female <NA>      alien purple     <NA>   dc comics <NA>       neutral  
## # ... with 14 more rows, and 1 more variable: weight <chr>
```

## `superhero_info`
3. Let's say we are only interested in the variables name, alignment, and "race". How would you isolate these variables from `superhero_info`?

```r
superhero_info %>% 
  select(name, alignment, race)
```

```
## # A tibble: 734 x 3
##    name          alignment race             
##    <chr>         <chr>     <chr>            
##  1 a-bomb        good      human            
##  2 abe sapien    good      icthyo sapien    
##  3 abin sur      good      ungaran          
##  4 abomination   bad       human / radiation
##  5 abraxas       bad       cosmic entity    
##  6 absorbing man bad       human            
##  7 adam monroe   good      <NA>             
##  8 adam strange  good      human            
##  9 agent 13      good      <NA>             
## 10 agent bob     good      human            
## # ... with 724 more rows
```

## Not Human
4. List all of the superheros that are not human.

```r
 superhero_info %>% 
  filter(race != "human")
```

```
## # A tibble: 222 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>      <chr>  <chr>     <chr>      <chr>    
##  1 abe ~ male   blue      icth~ no hair    191    dark hor~ blue       good     
##  2 abin~ male   blue      unga~ no hair    185    dc comics red        good     
##  3 abom~ male   green     huma~ no hair    203    marvel c~ <NA>       bad      
##  4 abra~ male   blue      cosm~ black      <NA>   marvel c~ <NA>       bad      
##  5 ajax  male   brown     cybo~ black      193    marvel c~ <NA>       bad      
##  6 alien male   <NA>      xeno~ no hair    244    dark hor~ black      bad      
##  7 amazo male   red       andr~ <NA>       257    dc comics <NA>       bad      
##  8 angel male   <NA>      vamp~ <NA>       <NA>   dark hor~ <NA>       good     
##  9 ange~ female yellow    muta~ black      165    marvel c~ <NA>       good     
## 10 anti~ male   yellow    god ~ no hair    61     dc comics <NA>       bad      
## # ... with 212 more rows, and 1 more variable: weight <chr>
```

## Good and Evil
5. Let's make two different data frames, one focused on the "good guys" and another focused on the "bad guys".

```r
good_guys <- superhero_info %>%
  filter(alignment == "good")
  
good_guys
```

```
## # A tibble: 496 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>      <chr>  <chr>     <chr>      <chr>    
##  1 a-bo~ male   yellow    human no hair    203    marvel c~ <NA>       good     
##  2 abe ~ male   blue      icth~ no hair    191    dark hor~ blue       good     
##  3 abin~ male   blue      unga~ no hair    185    dc comics red        good     
##  4 adam~ male   blue      <NA>  blond      <NA>   nbc - he~ <NA>       good     
##  5 adam~ male   blue      human blond      185    dc comics <NA>       good     
##  6 agen~ female blue      <NA>  blond      173    marvel c~ <NA>       good     
##  7 agen~ male   brown     human brown      178    marvel c~ <NA>       good     
##  8 agen~ male   <NA>      <NA>  <NA>       191    marvel c~ <NA>       good     
##  9 alan~ male   blue      <NA>  blond      180    dc comics <NA>       good     
## 10 alex~ male   <NA>      <NA>  <NA>       <NA>   nbc - he~ <NA>       good     
## # ... with 486 more rows, and 1 more variable: weight <chr>
```

```r
bad_guys <- superhero_info %>%
  filter(alignment == "bad")

bad_guys
```

```
## # A tibble: 207 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>      <chr>  <chr>     <chr>      <chr>    
##  1 abom~ male   green     huma~ no hair    203    marvel c~ <NA>       bad      
##  2 abra~ male   blue      cosm~ black      <NA>   marvel c~ <NA>       bad      
##  3 abso~ male   blue      human no hair    193    marvel c~ <NA>       bad      
##  4 air-~ male   blue      <NA>  white      188    marvel c~ <NA>       bad      
##  5 ajax  male   brown     cybo~ black      193    marvel c~ <NA>       bad      
##  6 alex~ male   <NA>      human <NA>       <NA>   wildstorm <NA>       bad      
##  7 alien male   <NA>      xeno~ no hair    244    dark hor~ black      bad      
##  8 amazo male   red       andr~ <NA>       257    dc comics <NA>       bad      
##  9 ammo  male   brown     human black      188    marvel c~ <NA>       bad      
## 10 ange~ female <NA>      <NA>  <NA>       <NA>   image co~ <NA>       bad      
## # ... with 197 more rows, and 1 more variable: weight <chr>
```

6. For the good guys, use the `tabyl` function to summarize their "race".

```r
good_guys %>% 
  tabyl(race)
```

```
##               race   n     percent valid_percent
##              alien   3 0.006048387   0.010752688
##              alpha   5 0.010080645   0.017921147
##             amazon   2 0.004032258   0.007168459
##            android   4 0.008064516   0.014336918
##             animal   2 0.004032258   0.007168459
##          asgardian   3 0.006048387   0.010752688
##          atlantean   4 0.008064516   0.014336918
##         bolovaxian   1 0.002016129   0.003584229
##              clone   1 0.002016129   0.003584229
##             cyborg   3 0.006048387   0.010752688
##           demi-god   2 0.004032258   0.007168459
##              demon   3 0.006048387   0.010752688
##            eternal   1 0.002016129   0.003584229
##     flora colossus   1 0.002016129   0.003584229
##        frost giant   1 0.002016129   0.003584229
##      god / eternal   6 0.012096774   0.021505376
##             gungan   1 0.002016129   0.003584229
##              human 148 0.298387097   0.530465950
##         human-kree   2 0.004032258   0.007168459
##      human-spartoi   1 0.002016129   0.003584229
##       human-vulcan   1 0.002016129   0.003584229
##    human-vuldarian   1 0.002016129   0.003584229
##    human / altered   2 0.004032258   0.007168459
##     human / cosmic   2 0.004032258   0.007168459
##  human / radiation   8 0.016129032   0.028673835
##      icthyo sapien   1 0.002016129   0.003584229
##            inhuman   4 0.008064516   0.014336918
##    kakarantharaian   1 0.002016129   0.003584229
##         kryptonian   4 0.008064516   0.014336918
##            martian   1 0.002016129   0.003584229
##          metahuman   1 0.002016129   0.003584229
##             mutant  46 0.092741935   0.164874552
##     mutant / clone   1 0.002016129   0.003584229
##             planet   1 0.002016129   0.003584229
##             saiyan   1 0.002016129   0.003584229
##           symbiote   3 0.006048387   0.010752688
##           talokite   1 0.002016129   0.003584229
##         tamaranean   1 0.002016129   0.003584229
##            ungaran   1 0.002016129   0.003584229
##            vampire   2 0.004032258   0.007168459
##     yoda's species   1 0.002016129   0.003584229
##      zen-whoberian   1 0.002016129   0.003584229
##               <NA> 217 0.437500000            NA
```

7. Among the good guys, Who are the Asgardians?

```r
good_guys %>% 
  filter(race == "asgardian")
```

```
## # A tibble: 3 x 10
##   name  gender eye_color race  hair_color height publisher skin_color alignment
##   <chr> <chr>  <chr>     <chr> <chr>      <chr>  <chr>     <chr>      <chr>    
## 1 sif   female blue      asga~ black      188    marvel c~ <NA>       good     
## 2 thor  male   blue      asga~ blond      198    marvel c~ <NA>       good     
## 3 thor~ female blue      asga~ blond      175    marvel c~ <NA>       good     
## # ... with 1 more variable: weight <chr>
```

8. Among the bad guys, who are the male humans over 200 inches in height?

```r
bad_guys %>% 
  filter(gender == "male" & race == "human" & height > 200)
```

```
## # A tibble: 5 x 10
##   name  gender eye_color race  hair_color height publisher skin_color alignment
##   <chr> <chr>  <chr>     <chr> <chr>      <chr>  <chr>     <chr>      <chr>    
## 1 bane  male   <NA>      human <NA>       203    dc comics <NA>       bad      
## 2 doct~ male   brown     human brown      201    marvel c~ <NA>       bad      
## 3 king~ male   blue      human no hair    201    marvel c~ <NA>       bad      
## 4 liza~ male   red       human no hair    203    marvel c~ <NA>       bad      
## 5 scor~ male   brown     human brown      211    marvel c~ <NA>       bad      
## # ... with 1 more variable: weight <chr>
```

9. OK, so are there more good guys or bad guys that are bald (personal interest)?

```r
names(superhero_info)
```

```
##  [1] "name"       "gender"     "eye_color"  "race"       "hair_color"
##  [6] "height"     "publisher"  "skin_color" "alignment"  "weight"
```

```r
superhero_info %>%
  filter(hair_color == "no hair") %>%
  filter(alignment == "good" | alignment == "bad") %>% 
  tabyl(alignment)
```

```
##  alignment  n   percent
##        bad 35 0.4861111
##       good 37 0.5138889
```



10. Let's explore who the really "big" superheros are. In the `superhero_info` data, which have a height over 200 or weight over 300?

```r
superhero_info %>%
  filter(height > 200 | weight > 300)
```

```
## # A tibble: 405 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>      <chr>  <chr>     <chr>      <chr>    
##  1 a-bo~ male   yellow    human no hair    203    marvel c~ <NA>       good     
##  2 abe ~ male   blue      icth~ no hair    191    dark hor~ blue       good     
##  3 abin~ male   blue      unga~ no hair    185    dc comics red        good     
##  4 abom~ male   green     huma~ no hair    203    marvel c~ <NA>       bad      
##  5 adam~ male   blue      human blond      185    dc comics <NA>       good     
##  6 agen~ female blue      <NA>  blond      173    marvel c~ <NA>       good     
##  7 agen~ male   brown     human brown      178    marvel c~ <NA>       good     
##  8 ajax  male   brown     cybo~ black      193    marvel c~ <NA>       bad      
##  9 alan~ male   blue      <NA>  blond      180    dc comics <NA>       good     
## 10 alfr~ male   blue      human black      178    dc comics <NA>       good     
## # ... with 395 more rows, and 1 more variable: weight <chr>
```

11. Just to be clear on the `|` operator,  have a look at the superheros over 300 in height...

```r
superhero_info %>%
  filter(height >300)
```

```
## # A tibble: 14 x 10
##    name  gender eye_color race  hair_color height publisher skin_color alignment
##    <chr> <chr>  <chr>     <chr> <chr>      <chr>  <chr>     <chr>      <chr>    
##  1 anti~ male   yellow    god ~ no hair    61     dc comics <NA>       bad      
##  2 fin ~ male   red       kaka~ no hair    975    marvel c~ green      good     
##  3 gala~ male   black     cosm~ black      876    marvel c~ <NA>       neutral  
##  4 giga~ female green     <NA>  red        62.5   dc comics <NA>       bad      
##  5 groot male   yellow    flor~ <NA>       701    marvel c~ <NA>       good     
##  6 howa~ male   brown     <NA>  yellow     79     marvel c~ <NA>       good     
##  7 jack~ male   blue      human brown      71     dark hor~ <NA>       good     
##  8 kryp~ male   blue      kryp~ white      64     dc comics <NA>       good     
##  9 modok male   white     cybo~ brownn     366    marvel c~ <NA>       bad      
## 10 onsl~ male   red       muta~ no hair    305    marvel c~ <NA>       bad      
## 11 sasq~ male   red       <NA>  orange     305    marvel c~ <NA>       good     
## 12 wolf~ female green     <NA>  auburn     366    marvel c~ <NA>       good     
## 13 ymir  male   white     fros~ no hair    304.8  marvel c~ white      good     
## 14 yoda  male   brown     yoda~ white      66     george l~ green      good     
## # ... with 1 more variable: weight <chr>
```

12. ...and the superheros over 450 in weight. Bonus question! Why do we not have 16 rows in question #10?

```r
superhero_info %>% 
  filter(height > 450)
```

```
## # A tibble: 9 x 10
##   name  gender eye_color race  hair_color height publisher skin_color alignment
##   <chr> <chr>  <chr>     <chr> <chr>      <chr>  <chr>     <chr>      <chr>    
## 1 anti~ male   yellow    god ~ no hair    61     dc comics <NA>       bad      
## 2 fin ~ male   red       kaka~ no hair    975    marvel c~ green      good     
## 3 gala~ male   black     cosm~ black      876    marvel c~ <NA>       neutral  
## 4 giga~ female green     <NA>  red        62.5   dc comics <NA>       bad      
## 5 groot male   yellow    flor~ <NA>       701    marvel c~ <NA>       good     
## 6 howa~ male   brown     <NA>  yellow     79     marvel c~ <NA>       good     
## 7 jack~ male   blue      human brown      71     dark hor~ <NA>       good     
## 8 kryp~ male   blue      kryp~ white      64     dc comics <NA>       good     
## 9 yoda  male   brown     yoda~ white      66     george l~ green      good     
## # ... with 1 more variable: weight <chr>
```
<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 20px;}
</style>
<div class = "blue">

## Height to Weight Ratio
13. It's easy to be strong when you are heavy and tall, but who is heavy and short? Which superheros have the highest height to weight ratio?

```r
#I couldnt get this to work correctly and I ran  out of time. I will come fix this later. 
#superhero_info %>% 
  #mutate(height_to_weight = height/weight) %>% 
  #arrange(height_to_weight)
```

</div>

## `superhero_powers`
Have a quick look at the `superhero_powers` data frame.  

```r
glimpse(superhero_powers)
```

```
## Rows: 667
## Columns: 168
## $ hero_names                   <chr> "3-D Man", "A-Bomb", "Abe Sapien", "Ab...
## $ agility                      <lgl> TRUE, FALSE, TRUE, FALSE, FALSE, FALSE...
## $ accelerated_healing          <lgl> FALSE, TRUE, TRUE, FALSE, TRUE, FALSE,...
## $ lantern_power_ring           <lgl> FALSE, FALSE, FALSE, TRUE, FALSE, FALS...
## $ dimensional_awareness        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ cold_resistance              <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALS...
## $ durability                   <lgl> FALSE, TRUE, TRUE, FALSE, FALSE, FALSE...
## $ stealth                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ energy_absorption            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ flight                       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ danger_sense                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ underwater_breathing         <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALS...
## $ marksmanship                 <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALS...
## $ weapons_master               <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALS...
## $ power_augmentation           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ animal_attributes            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ longevity                    <lgl> FALSE, TRUE, TRUE, FALSE, FALSE, FALSE...
## $ intelligence                 <lgl> FALSE, FALSE, TRUE, FALSE, TRUE, TRUE,...
## $ super_strength               <lgl> TRUE, TRUE, TRUE, FALSE, TRUE, TRUE, T...
## $ cryokinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ telepathy                    <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALS...
## $ energy_armor                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ energy_blasts                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ duplication                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ size_changing                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ density_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ stamina                      <lgl> TRUE, TRUE, TRUE, FALSE, TRUE, FALSE, ...
## $ astral_travel                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ audio_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ dexterity                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ omnitrix                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ super_speed                  <lgl> TRUE, FALSE, FALSE, FALSE, TRUE, TRUE,...
## $ possession                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ animal_oriented_powers       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ weapon_based_powers          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ electrokinesis               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ darkforce_manipulation       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ death_touch                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ teleportation                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ enhanced_senses              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ telekinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ energy_beams                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ magic                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ hyperkinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ jump                         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ clairvoyance                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ dimensional_travel           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ power_sense                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ shapeshifting                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ peak_human_condition         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ immortality                  <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, TRUE...
## $ camouflage                   <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALS...
## $ element_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ phasing                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ astral_projection            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ electrical_transport         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ fire_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ projection                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ summoning                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ enhanced_memory              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ reflexes                     <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALS...
## $ invulnerability              <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, TRUE...
## $ energy_constructs            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ force_fields                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ self_sustenance              <lgl> FALSE, TRUE, FALSE, FALSE, FALSE, FALS...
## $ anti_gravity                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ empathy                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ power_nullifier              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ radiation_control            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ psionic_powers               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ elasticity                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ substance_secretion          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ elemental_transmogrification <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ technopath_cyberpath         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ photographic_reflexes        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ seismic_power                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ animation                    <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, FALS...
## $ precognition                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ mind_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ fire_resistance              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ power_absorption             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ enhanced_hearing             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ nova_force                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ insanity                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ hypnokinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ animal_control               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ natural_armor                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ intangibility                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ enhanced_sight               <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALS...
## $ molecular_manipulation       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ heat_generation              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ adaptation                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ gliding                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ power_suit                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ mind_blast                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ probability_manipulation     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ gravity_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ regeneration                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ light_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ echolocation                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ levitation                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ toxin_and_disease_control    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ banish                       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ energy_manipulation          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ heat_resistance              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ natural_weapons              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ time_travel                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ enhanced_smell               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ illusions                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ thirstokinesis               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ hair_manipulation            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ illumination                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ omnipotent                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ cloaking                     <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ changing_armor               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ power_cosmic                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, TRU...
## $ biokinesis                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ water_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ radiation_immunity           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vision_telescopic            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ toxin_and_disease_resistance <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ spatial_awareness            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ energy_resistance            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ telepathy_resistance         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ molecular_combustion         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ omnilingualism               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ portal_creation              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ magnetism                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ mind_control_resistance      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ plant_control                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ sonar                        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ sonic_scream                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ time_manipulation            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ enhanced_touch               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ magic_resistance             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ invisibility                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ sub_mariner                  <lgl> FALSE, FALSE, TRUE, FALSE, FALSE, FALS...
## $ radiation_absorption         <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ intuitive_aptitude           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vision_microscopic           <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ melting                      <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ wind_control                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ super_breath                 <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, FALS...
## $ wallcrawling                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vision_night                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vision_infrared              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ grim_reaping                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ matter_absorption            <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ the_force                    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ resurrection                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ terrakinesis                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vision_heat                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vitakinesis                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ radar_sense                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ qwardian_power_ring          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ weather_control              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vision_x_ray                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vision_thermal               <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ web_creation                 <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ reality_warping              <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ odin_force                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ symbiote_costume             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ speed_force                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ phoenix_force                <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ molecular_dissipation        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ vision_cryo                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ omnipresent                  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
## $ omniscient                   <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FAL...
```

14. How many superheros have a combination of accelerated healing, durability, and super strength?

```r
superhero_powers %>% 
  filter(accelerated_healing == "TRUE" & durability == "TRUE" & super_strength == "TRUE") %>% 
  select(hero_names, accelerated_healing, durability, super_strength)
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
## # ... with 87 more rows
```

## `kinesis`
15. We are only interested in the superheros that do some kind of "kinesis". How would we isolate them from the `superhero_powers` data?

```r
superhero_powers %>%
  select(hero_names, contains("kinesis")) %>% 
  filter(thirstokinesis == "TRUE" | biokinesis == "TRUE" | terrakinesis == "TRUE" | vitakinesis == "TRUE" | electrokinesis == "TRUE" | telekinesis == "TRUE" | hyperkinesis == "TRUE" | hypnokinesis == "TRUE" | cryokinesis == "TRUE" | electrokinesis == "TRUE" | telekinesis == "TRUE" | hyperkinesis == "TRUE")
```

```
## # A tibble: 112 x 10
##    hero_names cryokinesis electrokinesis telekinesis hyperkinesis hypnokinesis
##    <chr>      <lgl>       <lgl>          <lgl>       <lgl>        <lgl>       
##  1 Alan Scott FALSE       FALSE          FALSE       FALSE        TRUE        
##  2 Amazo      TRUE        FALSE          FALSE       FALSE        FALSE       
##  3 Apocalypse FALSE       FALSE          TRUE        FALSE        FALSE       
##  4 Aqualad    TRUE        FALSE          FALSE       FALSE        FALSE       
##  5 Beyonder   FALSE       FALSE          TRUE        FALSE        FALSE       
##  6 Bizarro    TRUE        FALSE          FALSE       FALSE        TRUE        
##  7 Black Abb~ FALSE       FALSE          TRUE        FALSE        FALSE       
##  8 Black Adam FALSE       FALSE          TRUE        FALSE        FALSE       
##  9 Black Lig~ FALSE       TRUE           FALSE       FALSE        FALSE       
## 10 Black Mam~ FALSE       FALSE          FALSE       FALSE        TRUE        
## # ... with 102 more rows, and 4 more variables: thirstokinesis <lgl>,
## #   biokinesis <lgl>, terrakinesis <lgl>, vitakinesis <lgl>
```
	

16. Pick your favorite superhero and let's see their powers!

```r
superhero_powers %>% 
  filter(hero_names == "Luke Skywalker" )
```

```
## # A tibble: 1 x 168
##   hero_names agility accelerated_hea~ lantern_power_r~ dimensional_awa~
##   <chr>      <lgl>   <lgl>            <lgl>            <lgl>           
## 1 Luke Skyw~ TRUE    TRUE             FALSE            FALSE           
## # ... with 163 more variables: cold_resistance <lgl>, durability <lgl>,
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
## #   heat_resistance <lgl>, ...
```
