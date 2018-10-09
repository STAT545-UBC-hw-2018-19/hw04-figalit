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
ggplot(lifeExpPerYear_CanadaVsSwiss, aes(x=year, y=Canada)) +
  labs(title = "Life expectancy of Canada over time.") +
  labs(x = "Year") + 
  labs(y = "Canada's life expectancy") +
  geom_point()
```

![](assignment04-figalit_files/figure-markdown_github/unnamed-chunk-3-1.png)

The advantage of this approach lies in the fact that now we have the data in a readily and easily servable state for comparison with one another, and we can "combine" the two plots together to better see the underlying trends.

``` r
ggplot() +
  geom_point(data=lifeExpPerYear_CanadaVsSwiss, aes(year, Canada)) + 
  geom_smooth(data=lifeExpPerYear_CanadaVsSwiss, aes(year, Canada), 
              colour="darkblue", size=0.5) + 
  geom_point(data=lifeExpPerYear_CanadaVsSwiss, aes(year, Switzerland)) + 
  geom_smooth(data=lifeExpPerYear_CanadaVsSwiss, aes(year, Switzerland), 
              colour="pink", size=0.5)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](assignment04-figalit_files/figure-markdown_github/unnamed-chunk-4-1.png) In this case, it's very easily and clearly visible that Switzerland has a higher life expectancy than Canada over the years.

### Join Prompt (join, merge, look up)

In this prompt I have chosen Activity \#1:

\[x\] Create a second data frame, complementary to Gapminder. Join this with (part of) Gapminder using a dplyr join function and make some observations about the process and result. Explore the different types of joins. Examples of a second data frame you could build: \[x\] One row per country, a country variable and one or more variables with extra info, such as language spoken, NATO membership, national animal, or capitol city. \[x\] One row per continent, a continent variable and one or more variables with extra info, such as northern versus southern hemisphere.

Lets get a VERY QUICK feel of the gapminder dataset.

``` r
knitr::kable(head(gapminder))
```

| country       | continent    |     year|     lifeExp|          pop|                            gdpPercap|
|:--------------|:-------------|--------:|-----------:|------------:|------------------------------------:|
| Afghanistan   | Asia         |     1952|      28.801|      8425333|                             779.4453|
| Afghanistan   | Asia         |     1957|      30.332|      9240934|                             820.8530|
| Afghanistan   | Asia         |     1962|      31.997|     10267083|                             853.1007|
| Afghanistan   | Asia         |     1967|      34.020|     11537966|                             836.1971|
| Afghanistan   | Asia         |     1972|      36.088|     13079460|                             739.9811|
| Afghanistan   | Asia         |     1977|      38.438|     14880372|                             786.1134|
| And now let's | check what c |  ontinen|  ts there a|  re in gapmi|  nder, so our joins aren't far off..|

``` r
knitr::kable(unique(gapminder$continent))
```

| x        |
|:---------|
| Asia     |
| Europe   |
| Africa   |
| Americas |
| Oceania  |

I'll go ahead and build a super simple (and kind of proof of concept only) dataframe that has a row per continent, a variable about number of countries for that continent and another variable for whether it is in northern or southern hemisphere.

``` r
continentExtraInfo <- tribble( ~name, ~noCountries, ~hemisphere, 
                               "Asia", 48, "North",
                               "Europe", 44, "North",
                               "Africa", 54, "South",
                               "Americas", 55, "North",
                               "Oceania", 14, "South")
?tribble
```

    ## Help on topic 'tribble' was found in the following packages:
    ## 
    ##   Package               Library
    ##   tibble                /Library/Frameworks/R.framework/Versions/3.5/Resources/library
    ##   dplyr                 /Library/Frameworks/R.framework/Versions/3.5/Resources/library
    ## 
    ## 
    ## Using the first match ...

I could have alternatively built a csv file and read it into a dataframe, but due to the small data I avoided that approach, and use a nice tribble that makes more sense for such small data.

### Resources

Links I found useful: - <https://jules32.github.io/2016-07-12-Oxford/dplyr_tidyr/#3_tidyr_overview> - <https://stackoverflow.com/questions/21192002/how-to-combine-2-plots-ggplot-into-one-plot>
