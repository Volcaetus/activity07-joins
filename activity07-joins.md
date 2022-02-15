Activity 7
================
Name

## Data and packages

Load the entirety of the `{tidyverse}`. Be sure to avoid printing out
any unnecessary information and give the code chunk a meaningful name.

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✓ ggplot2 3.3.4     ✓ purrr   0.3.4
    ## ✓ tibble  3.1.2     ✓ dplyr   1.0.7
    ## ✓ tidyr   1.1.3     ✓ stringr 1.4.0
    ## ✓ readr   1.4.0     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

In this activity we will explore joining information that is contained
in multiple data files. We will also explore ways of visualizing spatial
data.

The late comedian [Mitch
Hedberg](https://en.wikipedia.org/wiki/Mitch_Hedberg) believed that La
Quinta (pronounced la KEEN-ta) was Spanish for “next to Denny’s”.

![Mitch
Hedberg](https://duckduckgrayduck.files.wordpress.com/2014/01/mitch-hedberg.jpg)

In the `data` folder, you have been provided with three `.csv` files.
The `dennys.csv` and `laquinta.csv` files contains addresses for the
locations of each respective company. The `states.csv` file contains the
area (thousand square miles) for each US state and the District of
Columbia. These data were scrapped from
[Denny’s](https://locations.dennys.com/) and [La
Quinta’s](https://www.wyndhamhotels.com/laquinta/locations) location web
pages by Mine Çetinkaya-Rundel.

Read each of the three files using `here::here` in combination with the
appropriate `{readr}` function. Assign each file to a meaningful object
name (e.g., `dennys`,`laquinta`, and `states`), be sure to avoid
printing any unnecessary information. Give your code chunk a meaningful
name.

``` r
setwd("~/STA518/Preps/activity07-joins")
d=read_csv(here::here('data','dennys.csv'))
```

    ## 
    ## ── Column specification ────────────────────────────────────────────────────────
    ## cols(
    ##   address = col_character(),
    ##   city = col_character(),
    ##   state = col_character(),
    ##   zip = col_character(),
    ##   longitude = col_double(),
    ##   latitude = col_double()
    ## )

``` r
l=read_csv(here::here('data','laquinta.csv'))
```

    ## 
    ## ── Column specification ────────────────────────────────────────────────────────
    ## cols(
    ##   address = col_character(),
    ##   city = col_character(),
    ##   state = col_character(),
    ##   zip = col_character(),
    ##   longitude = col_double(),
    ##   latitude = col_double()
    ## )

``` r
s=read_csv(here::here('data','states.csv'))
```

    ## 
    ## ── Column specification ────────────────────────────────────────────────────────
    ## cols(
    ##   name = col_character(),
    ##   abbreviation = col_character(),
    ##   area = col_double()
    ## )

### Provide more information

`class`, `str`, `nrow`, `ncol`, and `names` are extremely helpful
functions for quickly getting information about a dataset. Below is a
brief summary of the information that each function provides.

-   `class`: Returns the attribute of the R object
-   `str`: A compact display of the internal structure of an R object.
    This can also be viewed by clicking on the blue circle icon next to
    the R object in the **Environment** pane (upper-right pane).
-   `nrow`: Returns the number of rows in an R object
-   `ncol`: Returns the number of columns in an R object
-   `names`: Get or set the names of an R object (e.g., the column names
    for a dataset)

Use the output from `str` of these datasets to complete the `Type`
column in the partially started data dictionary tables below.

``` r
str(d)
```

    ## spec_tbl_df [1,643 × 6] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ address  : chr [1:1643] "2900 Denali" "3850 Debarr Road" "1929 Airport Way" "230 Connector Dr" ...
    ##  $ city     : chr [1:1643] "Anchorage" "Anchorage" "Fairbanks" "Auburn" ...
    ##  $ state    : chr [1:1643] "AK" "AK" "AK" "AL" ...
    ##  $ zip      : chr [1:1643] "99503" "99508" "99701" "36849" ...
    ##  $ longitude: num [1:1643] -149.9 -149.8 -147.8 -85.5 -86.8 ...
    ##  $ latitude : num [1:1643] 61.2 61.2 64.8 32.6 33.6 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   address = col_character(),
    ##   ..   city = col_character(),
    ##   ..   state = col_character(),
    ##   ..   zip = col_character(),
    ##   ..   longitude = col_double(),
    ##   ..   latitude = col_double()
    ##   .. )

``` r
str(l)
```

    ## spec_tbl_df [909 × 6] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ address  : chr [1:909] "793 W. Bel Air Avenue" "3018 CatClaw Dr" "3501 West Lake Rd" "184 North Point Way" ...
    ##  $ city     : chr [1:909] "\nAberdeen" "\nAbilene" "\nAbilene" "\nAcworth" ...
    ##  $ state    : chr [1:909] "MD" "TX" "TX" "GA" ...
    ##  $ zip      : chr [1:909] "21001" "79606" "79601" "30102" ...
    ##  $ longitude: num [1:909] -76.2 -99.8 -99.7 -84.7 -96.6 ...
    ##  $ latitude : num [1:909] 39.5 32.4 32.5 34.1 34.8 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   address = col_character(),
    ##   ..   city = col_character(),
    ##   ..   state = col_character(),
    ##   ..   zip = col_character(),
    ##   ..   longitude = col_double(),
    ##   ..   latitude = col_double()
    ##   .. )

``` r
str(s)
```

    ## spec_tbl_df [51 × 3] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ name        : chr [1:51] "Alabama" "Alaska" "Arizona" "Arkansas" ...
    ##  $ abbreviation: chr [1:51] "AL" "AK" "AZ" "AR" ...
    ##  $ area        : num [1:51] 52420 665384 113990 53179 163695 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   name = col_character(),
    ##   ..   abbreviation = col_character(),
    ##   ..   area = col_double()
    ##   .. )

#### Denny’s data

| Variable    | Type | Brief description                 |
|-------------|------|-----------------------------------|
| `address`   |      | street address of dennys location |
| `city`      |      | city of dennys location           |
| `state`     |      | state of dennys location          |
| `zip`       |      | zip code of dennys location       |
| `longitude` |      | east-west position on Earth       |
| `latitude`  |      | north-south position on Earth     |

#### La Quinta’s data

| Variable    | Type | Brief description                   |
|-------------|------|-------------------------------------|
| `address`   |      | street address of laquinta location |
| `city`      |      | city of laquinta location           |
| `state`     |      | state of laquinta location          |
| `zip`       |      | zip code of laquinta location       |
| `longitude` |      | east-west position on Earth         |
| `latitude`  |      | north-south position on Earth       |

#### States data

| Variable       | Type | Brief description             |
|----------------|------|-------------------------------|
| `name`         |      | state name                    |
| `abbreviation` |      | state abbreviation            |
| `area`         |      | area in thousand square miles |

![](README-img/noun_pause.png) **Planned Pause Point**: If you have any
questions, contact your instructor. Otherwise feel free to continue on.

## Analysis

### Locations in the US

We will limit our analysis to Denny’s and La Quinta’s locations within
the United States Look at the websites that the data come from (linked
above). Are there any La Quinta’s locations outside of the US? What
about Denny’s?

**Response**:

yup

If we wanted to do this using the datasets, would we need to ? Don’t
worry about implementing this yet, you only need to brainstorm some
ideas. Include at least one idea as your answer, but you are welcome to
write down a few options too.

**Response**: join on the state data set from both dennys and laquinta

### Preparing to Join

#### Denny’s

Now we will find the Denny’s locations that are outside the US using
code. To do so, *filter* the Denny’s locations for observations where
state is *not in* `states$abbreviation`. Do not assign this to anything;
we only want to see if we need to be aware of non-US cases. If there are
any non-US locations, specify where these are.

``` r
try = d %>%
  filter(!d$state %in% (s$abbreviation))

try
```

    ## # A tibble: 0 x 6
    ## # … with 6 variables: address <chr>, city <chr>, state <chr>, zip <chr>,
    ## #   longitude <dbl>, latitude <dbl>

``` r
s%>%distinct(s$abbreviation)
```

    ## # A tibble: 51 x 1
    ##    `s$abbreviation`
    ##    <chr>           
    ##  1 AL              
    ##  2 AK              
    ##  3 AZ              
    ##  4 AR              
    ##  5 CA              
    ##  6 CO              
    ##  7 CT              
    ##  8 DE              
    ##  9 FL              
    ## 10 GA              
    ## # … with 41 more rows

**Response**: it appears none in dennys

Now do this again, but using `anti_join`. To do so, take the Denny’s
locations and anti-join this with the states dataset. Remember to
specify your `by` columns.

``` r
try1=s %>% 
  anti_join(d,by=c("abbreviation"="state"))
try1
```

    ## # A tibble: 0 x 3
    ## # … with 3 variables: name <chr>, abbreviation <chr>, area <dbl>

#### A Brief Aside

Another way to do this would be to create a new variable (called, says,
`country`), then filter on this new variable. Remember that `mutate`
creates new variables. Then `dplyr::case_when` function is a nice way to
do multiple if-else statements. For example, your instructor could see
if there are any Denny’s in any the states that your instructor has
lived in:

``` r
dd=d %>%
  mutate(bradford_lived = case_when(
    state %in% c("MI", "NC", "IL", "ME") ~ "Yes",
    TRUE ~ "No"))
```

`case_when` looks to see if any of the `dennys$state`s are in the vector
of state abbreviations that I have lived (i.e.,
`c("MI", "NC", "IL", "ME")`). If this is a `TRUE` statement, then
`bradford_lived` will be set to `"Yes"`. Otherwise, if the values is not
a missing value (i.e., `NA`), the `TRUE ~ "NO"` line sets
`bradford_lived` to `"No`. If the values are missing, they remain
missing in `bradford_lived`. Without the `TRUE ~ "No"` line, all values
except for “MI”, “NC”, “IL”, and “ME” would be set to missing values.

Note that we could have also used
`!(state %in% c("MI", "NC", "IL", "ME")) ~ "NO"`. However, this would be
slightly inefficient to type as `TRUE ~ "No"` captures this.

To create a new variable called `country` where if the state is in the
US, `country` is set to `"United States"` we would then do:

``` r
ddd=d %>% 
  mutate(country = case_when(
    state %in% s$abbreviation ~ "United States",
    TRUE ~ "Other")) %>% 
  filter(country == "Other")
```

![](README-img/noun_pause.png) **Planned Pause Point**: If you have any
questions, contact your instructor. Otherwise feel free to continue on.

#### La Quinta

Determine if La Quinta has any locations that are outside of the US.

``` r
ll=l %>% 
  mutate(country = case_when(
    state %in% s$abbreviation ~ "United States",
    TRUE ~ "Other")) %>% 
  filter(country == "Other")
```

``` r
try2 = l %>%
  anti_join(s,by=c('state'='abbreviation'))
try2
```

    ## # A tibble: 14 x 6
    ##    address                     city               state zip   longitude latitude
    ##    <chr>                       <chr>              <chr> <chr>     <dbl>    <dbl>
    ##  1 Carretera Panamericana Sur… "\nAguascalientes" AG    20345    -102.     21.8 
    ##  2 Av. Tulum Mza. 14 S.M. 4 L… "\nCancun"         QR    77500     -86.8    21.2 
    ##  3 Ejercito Nacional 8211      "Col\nPartido Igl… CH    32528    -106.     31.7 
    ##  4 Blvd. Aeropuerto 4001       "Parque Industria… NL    66600    -100.     25.8 
    ##  5 Carrera 38 # 26-13 Avenida… "\nMedellin Colom… ANT   0500…     -75.6     6.22
    ##  6 AV. PINO SUAREZ No. 1001    "Col. Centro\nMon… NL    64000    -100.     25.7 
    ##  7 Av. Fidel Velazquez #3000 … "\nMonterrey"      NL    64190    -100.     25.7 
    ##  8 63 King Street East         "\nOshawa"         ON    L1H1…     -78.9    43.9 
    ##  9 Calle Las Torres-1 Colonia… "\nPoza Rica"      VE    93210     -97.4    20.6 
    ## 10 Blvd. Audi N. 3 Ciudad Mod… "\nSan Jose Chiap… PU    75010     -97.8    19.2 
    ## 11 Ave. Zeta del Cochero No 4… "Col. ReservaTerr… PU    72810     -98.2    19.0 
    ## 12 Av. Benito Juarez 1230 B (… "\nSan Luis Potos… SL    78399    -101.     22.1 
    ## 13 Blvd. Fuerza Armadas        "contiguo Mall La… FM    11101     -87.2    14.1 
    ## 14 8640 Alexandra Rd           "\nRichmond"       BC    V6X1…    -123.     49.2

#### Isolating US locations

For the rest of this activity, we will work with the data from the
United States *only*. All Denny’s locations in our file are in the US so
we do not need to worry about updating this object, but you do need to
do some work on the `laquinta` data. Create a new object called
`laquinta_us` that only contains the locations inside the US.

``` r
t=l %>% 
  mutate(country = case_when(
    state %in% s$abbreviation ~ "United States",
    TRUE ~ "Other")) %>% 
  filter(country != "Other")
t
```

    ## # A tibble: 895 x 7
    ##    address                city         state zip   longitude latitude country   
    ##    <chr>                  <chr>        <chr> <chr>     <dbl>    <dbl> <chr>     
    ##  1 793 W. Bel Air Avenue  "\nAberdeen" MD    21001     -76.2     39.5 United St…
    ##  2 3018 CatClaw Dr        "\nAbilene"  TX    79606     -99.8     32.4 United St…
    ##  3 3501 West Lake Rd      "\nAbilene"  TX    79601     -99.7     32.5 United St…
    ##  4 184 North Point Way    "\nAcworth"  GA    30102     -84.7     34.1 United St…
    ##  5 2828 East Arlington S… "\nAda"      OK    74820     -96.6     34.8 United St…
    ##  6 14925 Landmark Blvd    "\nAddison"  TX    75254     -96.8     33.0 United St…
    ##  7 909 East Frontage Rd   "\nAlamo"    TX    78516     -98.1     26.2 United St…
    ##  8 2116 Yale Blvd Southe… "\nAlbuquer… NM    87106    -107.      35.1 United St…
    ##  9 7439 Pan American Fwy… "\nAlbuquer… NM    87109    -107.      35.2 United St…
    ## 10 2011 Menaul Blvd Nort… "\nAlbuquer… NM    87107    -107.      35.1 United St…
    ## # … with 885 more rows

``` r
tt=d %>% 
  mutate(country = case_when(
    state %in% s$abbreviation ~ "United States",
    TRUE ~ "Other")) %>% 
  filter(country != "Other")
tt
```

    ## # A tibble: 1,643 x 7
    ##    address                city         state zip   longitude latitude country   
    ##    <chr>                  <chr>        <chr> <chr>     <dbl>    <dbl> <chr>     
    ##  1 2900 Denali            Anchorage    AK    99503    -150.      61.2 United St…
    ##  2 3850 Debarr Road       Anchorage    AK    99508    -150.      61.2 United St…
    ##  3 1929 Airport Way       Fairbanks    AK    99701    -148.      64.8 United St…
    ##  4 230 Connector Dr       Auburn       AL    36849     -85.5     32.6 United St…
    ##  5 224 Daniel Payne Driv… Birmingham   AL    35207     -86.8     33.6 United St…
    ##  6 900 16th St S, Common… Birmingham   AL    35294     -86.8     33.5 United St…
    ##  7 5931 Alabama Highway,… Cullman      AL    35056     -86.9     34.2 United St…
    ##  8 2190 Ross Clark Circle Dothan       AL    36301     -85.4     31.2 United St…
    ##  9 900 Tyson Rd           Hope Hull (… AL    36043     -86.4     32.2 United St…
    ## 10 4874 University Drive  Huntsville   AL    35816     -86.7     34.7 United St…
    ## # … with 1,633 more rows

### Fewest locations

Let’s test some of our data summary skills.

Which US state(s) has/ve the fewest Denny’s location?

``` r
a = count(tt,state)
a
```

    ## # A tibble: 51 x 2
    ##    state     n
    ##    <chr> <int>
    ##  1 AK        3
    ##  2 AL        7
    ##  3 AR        9
    ##  4 AZ       83
    ##  5 CA      403
    ##  6 CO       29
    ##  7 CT       12
    ##  8 DC        2
    ##  9 DE        1
    ## 10 FL      140
    ## # … with 41 more rows

**Response**: DE

Which US state(s) has/ve the fewest La Quinta locations?

``` r
a = count(t,state)
a
```

    ## # A tibble: 48 x 2
    ##    state     n
    ##    <chr> <int>
    ##  1 AK        2
    ##  2 AL       16
    ##  3 AR       13
    ##  4 AZ       18
    ##  5 CA       56
    ##  6 CO       27
    ##  7 CT        6
    ##  8 FL       74
    ##  9 GA       41
    ## 10 IA        4
    ## # … with 38 more rows

**Response**: ME

Is this surprising to you? Why or why not?

**Response**: dunno never bee to either to know enuf about them

### Locations per thousand square miles

Next we will calculate which states have the most Denny’s and La Quinta
locations per thousand square miles. To do this, we will need to join
each company’s tibble with the information in the `states` tibble.

#### Denny’s

1.  Take the `dennys` tibble and determine how many observations are in
    each state. This should have two columns: `state` and `n`.
2.  *Then*, use `inner_join` to join the previous information to the
    `states` tibble. Note that the variables in the `states` tibble has
    the two letter abbreviations in a column called `abbreviation`.
    Therefore, we will need to specify that the `state` variable from
    the `dennys` tibble should be matched `by` the `abbreviation`
    variable from the `states` tibble.
3.  *Then*, calculate the number of Denny’s locations *per* thousand
    square miles.

``` r
s = rename(s, state = abbreviation)
s
```

    ## # A tibble: 51 x 3
    ##    name        state    area
    ##    <chr>       <chr>   <dbl>
    ##  1 Alabama     AL     52420.
    ##  2 Alaska      AK    665384.
    ##  3 Arizona     AZ    113990.
    ##  4 Arkansas    AR     53179.
    ##  5 California  CA    163695.
    ##  6 Colorado    CO    104094.
    ##  7 Connecticut CT      5543.
    ##  8 Delaware    DE      2489.
    ##  9 Florida     FL     65758.
    ## 10 Georgia     GA     59425.
    ## # … with 41 more rows

``` r
b = count(t,state)
b
```

    ## # A tibble: 48 x 2
    ##    state     n
    ##    <chr> <int>
    ##  1 AK        2
    ##  2 AL       16
    ##  3 AR       13
    ##  4 AZ       18
    ##  5 CA       56
    ##  6 CO       27
    ##  7 CT        6
    ##  8 FL       74
    ##  9 GA       41
    ## 10 IA        4
    ## # … with 38 more rows

``` r
bb = s%>%
  left_join(b,by='state')

bb
```

    ## # A tibble: 51 x 4
    ##    name        state    area     n
    ##    <chr>       <chr>   <dbl> <int>
    ##  1 Alabama     AL     52420.    16
    ##  2 Alaska      AK    665384.     2
    ##  3 Arizona     AZ    113990.    18
    ##  4 Arkansas    AR     53179.    13
    ##  5 California  CA    163695.    56
    ##  6 Colorado    CO    104094.    27
    ##  7 Connecticut CT      5543.     6
    ##  8 Delaware    DE      2489.    NA
    ##  9 Florida     FL     65758.    74
    ## 10 Georgia     GA     59425.    41
    ## # … with 41 more rows

``` r
bb$persqmi = (bb$n / bb$area)*1000
bb
```

    ## # A tibble: 51 x 5
    ##    name        state    area     n  persqmi
    ##    <chr>       <chr>   <dbl> <int>    <dbl>
    ##  1 Alabama     AL     52420.    16  0.305  
    ##  2 Alaska      AK    665384.     2  0.00301
    ##  3 Arizona     AZ    113990.    18  0.158  
    ##  4 Arkansas    AR     53179.    13  0.244  
    ##  5 California  CA    163695.    56  0.342  
    ##  6 Colorado    CO    104094.    27  0.259  
    ##  7 Connecticut CT      5543.     6  1.08   
    ##  8 Delaware    DE      2489.    NA NA      
    ##  9 Florida     FL     65758.    74  1.13   
    ## 10 Georgia     GA     59425.    41  0.690  
    ## # … with 41 more rows

Which states have the most Denny’s locations per thousand square miles?

**Response**: RI

#### La Quinta

Similarly as we previously did for Denny’s, calculate the number of La
Quinta locations *per* thousand square miles.

**Response**: meh

<img src="README-img/noun_pause.png" alt="pause" width = "20"/>
<b>Planned Pause Point</b>: If you feel that you have a good
understanding of these commands, feel free to start working on your
project. The remainder of this activity will deal with creating map data
visualizations.

### Mapping locations

To be able to map all of the `dennys` and `laquinta` locations, we
should combine them into one dataset.

Both of these tibbles have the same columns in the same order (i.e.,
`address`, `city`, `state`, `zip`, `longitude`, `latitude`, `country`)
so we can simply stack them on top of each other by using `bind_rows`.

Why would it not be wise to use a mutating join (e.g., `inner_join`,
`left/right_join`, `full_join`) for these data? That is, both of the
datasets have the same variable names, would it not make sense to join
them at the same address?

**Response**:

If we were able to use a mutating join, what would this output would
look like? Why would this not be helpful?

**Response**:

You were already shown these top-notch animations from [Garrick
Aden-Blue](https://www.garrickadenbuie.com/project/tidyexplain/). Again,
they are a little old as they still use `spread` and `gather`, but the
`*_join` are still accurate.

Prior to combining these datasets, it would be nice to know which
company each location belongs to.

``` r
pre_join_dennys <- dennys %>% 
  mutate(establishment = "Denny's")
```

    ## Error in mutate(., establishment = "Denny's"): object 'dennys' not found

``` r
pre_join_laquinta_us <- laquinta_us %>% 
  mutate(establishment = "La Quinta")
```

    ## Error in mutate(., establishment = "La Quinta"): object 'laquinta_us' not found

Now, stack these two `pre_join_*` tibbles on top of each other. After
you have verified the stacking worked, assign the resulting object to
`dennys_laquinta`.

We can plot the locations of the two establishments using a scatter plot
and color the points by the establishment type. Note that longitude
should be plotted on the *x*-axis and latitude on the *y*-axis.

**Response**:

### More with visualizing

The following two items ask you to create visualizations. These should
follow best practices such as informative titles, axis labels, etc. See
<http://ggplot2.tidyverse.org/reference/labs.html> for help with the
syntax.

You can also choose different themes to change the overall look of your
plots. See <http://ggplot2.tidyverse.org/reference/ggtheme.html> for
help with these.

The general idea here is that create the map using a dataset of shape
files, then overlay a layer of points using a different dataset.

Note that [choropleth
maps](https://r-charts.com/spatial/choropleth-map-ggplot2/) are slightly
different in that you would need to joining the shape file and the data
containing ammounts to shade each shape.

#### Michigan locations

Filter the data for observations in Michigan only, and create a plot.
Try adjusting the transparency of the points by setting the `alpha`
level (so that it is easier to see the over-plotted ones). Visually,
does Mitch Hedberg’s joke appear to hold in Michigan?

**Response**:

#### Texas locations

Now filter the data for observations in Texas only. Create the plot,
with an appropriate `alpha` level. Visually, does Mitch Hedberg’s joke
appear to hold here?

**Response**:

### Challenge: Dress up your maps

Hadley’s [ggplot2](https://ggplot2-book.org/maps.html) text shows how to
use the `geom_polygon` and `coord_quickmap` layers to create state
outlines when plotting spatial data.

This blog post series on **r-spatial** goes into a lot more detail for
dressing up maps:

-   [Basics](https://www.r-spatial.org/r/2018/10/25/ggplot2-sf.html)
-   [Layers](https://www.r-spatial.org/r/2018/10/25/ggplot2-sf-2.html)
-   [Layout](https://www.r-spatial.org/r/2018/10/25/ggplot2-sf-3.html)

Add state boundaries to each of your maps. Do this within each of the
previous code chunks.

![](README-img/noun_pause.png) **Planned Pause Point**: If you have any
questions, contact your instructor. Otherwise feel free to continue on.

## Attribution

Inspiration for this Activity was provided by [Mine
Çetinkaya-Rundel](http://www2.stat.duke.edu/courses/Spring18/Sta199/).
