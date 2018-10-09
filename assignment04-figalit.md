Assignment 04
================
Figali Taho
09 Oct 2018

-   [Tidy data and joins!](#tidy-data-and-joins)
    -   [Data Reshaping Prompt](#data-reshaping-prompt)
    -   [Join Prompt (join, merge, look up)](#join-prompt-join-merge-look-up)
    -   [Resources](#resources)

Tidy data and joins!
--------------------

For this assignment, I will work with the Gapminder dataset again.

### Data Reshaping Prompt

In this prompt I have chosen Activity \#2:

\[x\] Make a tibble with one row per year and columns for life expectancy for two or more countries. Use knitr::kable() to make this table look pretty in your rendered homework. - I will explore the life expectancy of Canada and Switzerland.

``` r
filteredCanSwiss <- gapminder %>%
  filter(country %in% c("Canada", "Switzerland")) %>%
  select(year, country, lifeExp)
lifeExpPerYear_CanadaVsSwiss <- spread(filteredCanSwiss, country, lifeExp)
```

We have now brought the data to the required state, where there is one line per year, and Canada's and Switzerland's life expectancy as columns:

``` r
knitr::kable(lifeExpPerYear_CanadaVsSwiss)
```

|  year|  Canada|  Switzerland|
|-----:|-------:|------------:|
|  1952|  68.750|       69.620|
|  1957|  69.960|       70.560|
|  1962|  71.300|       71.320|
|  1967|  72.130|       72.770|
|  1972|  72.880|       73.780|
|  1977|  74.210|       75.390|
|  1982|  75.760|       76.210|
|  1987|  76.860|       77.410|
|  1992|  77.950|       78.030|
|  1997|  78.610|       79.370|
|  2002|  79.770|       80.620|
|  2007|  80.653|       81.701|

\[x\] Take advantage of this new data shape to scatterplot life expectancy for one country against that of another. To plot the data for each country separately, we would do something on these terms, for example, for Canada:

``` r
ggplot(lifeExpPerYear_CanadaVsSwiss, aes(x=year, y=Canada, ylab="Canada's life expectancy")) +
  geom_point()
```

![](assignment04-figalit_files/figure-markdown_github/unnamed-chunk-3-1.png)

### Join Prompt (join, merge, look up)

In this prompt I have chosen Activity \#1:

\[x\] Create a second data frame, complementary to Gapminder. Join this with (part of) Gapminder using a dplyr join function and make some observations about the process and result. Explore the different types of joins. Examples of a second data frame you could build: \[x\] One row per country, a country variable and one or more variables with extra info, such as language spoken, NATO membership, national animal, or capitol city. \[x\] One row per continent, a continent variable and one or more variables with extra info, such as northern versus southern hemisphere.

### Resources

Links I found useful: <https://jules32.github.io/2016-07-12-Oxford/dplyr_tidyr/#3_tidyr_overview>
