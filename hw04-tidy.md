hw04-tidy
================

### Data reshaping

##### Activity 2

Make a tibble with one row per year and columns for life expectancy for two or more countries.

Use knitr::kable() to make this table look pretty in your rendered homework.

Take advantage of this new data shape to scatterplot life expectancy for one country against that of another.

``` r
suppressPackageStartupMessages(library("gapminder"))
suppressPackageStartupMessages(library("tidyverse"))
```

``` r
gapminder_tidy <- gapminder %>% 
  filter(country == "Canada" | country == "United States") %>% 
  select(country, year, lifeExp) %>% 
  group_by(year) %>% 
  spread(key = "country", value = "lifeExp")
knitr::kable(gapminder_tidy)
```

|  year|  Canada|  United States|
|-----:|-------:|--------------:|
|  1952|  68.750|         68.440|
|  1957|  69.960|         69.490|
|  1962|  71.300|         70.210|
|  1967|  72.130|         70.760|
|  1972|  72.880|         71.340|
|  1977|  74.210|         73.380|
|  1982|  75.760|         74.650|
|  1987|  76.860|         75.020|
|  1992|  77.950|         76.090|
|  1997|  78.610|         76.810|
|  2002|  79.770|         77.310|
|  2007|  80.653|         78.242|

``` r
gapminder_tidy %>% 
  ggplot(aes(Canada, `United States`)) +
  geom_point()
```

![](hw04-tidy_files/figure-markdown_github/plot_lifeExp_Canada_vs_US-1.png)

Over the years 1952-2007, life expectancy in both countries (Canada and US) was increasing. We can observe a positive correlation in the scatterplot. Overall, Canada had slightly higher life expectancy than US.

### Joining tables

##### Activity 1

Create a second data frame, complementary to Gapminder. Join this with (part of) Gapminder using a dplyr join function and make some observations about the process and result. Explore the different types of joins.

``` r
countries_18 <- tibble(country = c("Canada",
                                "Australia",
                                "New Zealand",
                                "United States",
                                "Mexico",
                                "France",
                                "Norway",
                                "Greece",
                                "China",
                                "Japan",
                                "Turkey",
                                "Zimbabwe",
                                "Uruguay",
                                "Brazil",
                                "Somalia",
                                "Thailand",
                                "Ethiopia",
                                "Russia"),
                    capital = c("Ottawa",
                                "Canberra",
                                "Wellington",
                                "Washington",
                                "Mexico City",
                                "Paris",
                                "Oslo",
                                "Athens",
                                "Beijing",
                                "Tokyo",
                                "Ankara",
                                "Harare",
                                "Montevideo",
                                "Brasilia",
                                "Mogadishu",
                                "Bangkok",
                                "Addis Ababa",
                                "Moscow"))
```

The tibble consists of 18 (random) countries and an additional information about them (capital) not mentioned in the gapminder data set.

``` r
joined_inner <- inner_join(gapminder, countries_18)
```

    ## Joining, by = "country"

    ## Warning: Column `country` joining factor and character vector, coercing
    ## into character vector

``` r
str(joined_inner)
```

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    204 obs. of  7 variables:
    ##  $ country  : chr  "Australia" "Australia" "Australia" "Australia" ...
    ##  $ continent: Factor w/ 5 levels "Africa","Americas",..: 5 5 5 5 5 5 5 5 5 5 ...
    ##  $ year     : int  1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
    ##  $ lifeExp  : num  69.1 70.3 70.9 71.1 71.9 ...
    ##  $ pop      : int  8691212 9712569 10794968 11872264 13177000 14074100 15184200 16257249 17481977 18565243 ...
    ##  $ gdpPercap: num  10040 10950 12217 14526 16789 ...
    ##  $ capital  : chr  "Canberra" "Canberra" "Canberra" "Canberra" ...

``` r
levels(as.factor(joined_inner$country))
```

    ##  [1] "Australia"     "Brazil"        "Canada"        "China"        
    ##  [5] "Ethiopia"      "France"        "Greece"        "Japan"        
    ##  [9] "Mexico"        "New Zealand"   "Norway"        "Somalia"      
    ## [13] "Thailand"      "Turkey"        "United States" "Uruguay"      
    ## [17] "Zimbabwe"

``` r
levels(as.factor(joined_inner$year))
```

    ##  [1] "1952" "1957" "1962" "1967" "1972" "1977" "1982" "1987" "1992" "1997"
    ## [11] "2002" "2007"

``` r
knitr::kable(joined_inner)
```

| country       | continent |  year|   lifeExp|         pop|   gdpPercap| capital     |
|:--------------|:----------|-----:|---------:|-----------:|-----------:|:------------|
| Australia     | Oceania   |  1952|  69.12000|     8691212|  10039.5956| Canberra    |
| Australia     | Oceania   |  1957|  70.33000|     9712569|  10949.6496| Canberra    |
| Australia     | Oceania   |  1962|  70.93000|    10794968|  12217.2269| Canberra    |
| Australia     | Oceania   |  1967|  71.10000|    11872264|  14526.1246| Canberra    |
| Australia     | Oceania   |  1972|  71.93000|    13177000|  16788.6295| Canberra    |
| Australia     | Oceania   |  1977|  73.49000|    14074100|  18334.1975| Canberra    |
| Australia     | Oceania   |  1982|  74.74000|    15184200|  19477.0093| Canberra    |
| Australia     | Oceania   |  1987|  76.32000|    16257249|  21888.8890| Canberra    |
| Australia     | Oceania   |  1992|  77.56000|    17481977|  23424.7668| Canberra    |
| Australia     | Oceania   |  1997|  78.83000|    18565243|  26997.9366| Canberra    |
| Australia     | Oceania   |  2002|  80.37000|    19546792|  30687.7547| Canberra    |
| Australia     | Oceania   |  2007|  81.23500|    20434176|  34435.3674| Canberra    |
| Brazil        | Americas  |  1952|  50.91700|    56602560|   2108.9444| Brasilia    |
| Brazil        | Americas  |  1957|  53.28500|    65551171|   2487.3660| Brasilia    |
| Brazil        | Americas  |  1962|  55.66500|    76039390|   3336.5858| Brasilia    |
| Brazil        | Americas  |  1967|  57.63200|    88049823|   3429.8644| Brasilia    |
| Brazil        | Americas  |  1972|  59.50400|   100840058|   4985.7115| Brasilia    |
| Brazil        | Americas  |  1977|  61.48900|   114313951|   6660.1187| Brasilia    |
| Brazil        | Americas  |  1982|  63.33600|   128962939|   7030.8359| Brasilia    |
| Brazil        | Americas  |  1987|  65.20500|   142938076|   7807.0958| Brasilia    |
| Brazil        | Americas  |  1992|  67.05700|   155975974|   6950.2830| Brasilia    |
| Brazil        | Americas  |  1997|  69.38800|   168546719|   7957.9808| Brasilia    |
| Brazil        | Americas  |  2002|  71.00600|   179914212|   8131.2128| Brasilia    |
| Brazil        | Americas  |  2007|  72.39000|   190010647|   9065.8008| Brasilia    |
| Canada        | Americas  |  1952|  68.75000|    14785584|  11367.1611| Ottawa      |
| Canada        | Americas  |  1957|  69.96000|    17010154|  12489.9501| Ottawa      |
| Canada        | Americas  |  1962|  71.30000|    18985849|  13462.4855| Ottawa      |
| Canada        | Americas  |  1967|  72.13000|    20819767|  16076.5880| Ottawa      |
| Canada        | Americas  |  1972|  72.88000|    22284500|  18970.5709| Ottawa      |
| Canada        | Americas  |  1977|  74.21000|    23796400|  22090.8831| Ottawa      |
| Canada        | Americas  |  1982|  75.76000|    25201900|  22898.7921| Ottawa      |
| Canada        | Americas  |  1987|  76.86000|    26549700|  26626.5150| Ottawa      |
| Canada        | Americas  |  1992|  77.95000|    28523502|  26342.8843| Ottawa      |
| Canada        | Americas  |  1997|  78.61000|    30305843|  28954.9259| Ottawa      |
| Canada        | Americas  |  2002|  79.77000|    31902268|  33328.9651| Ottawa      |
| Canada        | Americas  |  2007|  80.65300|    33390141|  36319.2350| Ottawa      |
| China         | Asia      |  1952|  44.00000|   556263527|    400.4486| Beijing     |
| China         | Asia      |  1957|  50.54896|   637408000|    575.9870| Beijing     |
| China         | Asia      |  1962|  44.50136|   665770000|    487.6740| Beijing     |
| China         | Asia      |  1967|  58.38112|   754550000|    612.7057| Beijing     |
| China         | Asia      |  1972|  63.11888|   862030000|    676.9001| Beijing     |
| China         | Asia      |  1977|  63.96736|   943455000|    741.2375| Beijing     |
| China         | Asia      |  1982|  65.52500|  1000281000|    962.4214| Beijing     |
| China         | Asia      |  1987|  67.27400|  1084035000|   1378.9040| Beijing     |
| China         | Asia      |  1992|  68.69000|  1164970000|   1655.7842| Beijing     |
| China         | Asia      |  1997|  70.42600|  1230075000|   2289.2341| Beijing     |
| China         | Asia      |  2002|  72.02800|  1280400000|   3119.2809| Beijing     |
| China         | Asia      |  2007|  72.96100|  1318683096|   4959.1149| Beijing     |
| Ethiopia      | Africa    |  1952|  34.07800|    20860941|    362.1463| Addis Ababa |
| Ethiopia      | Africa    |  1957|  36.66700|    22815614|    378.9042| Addis Ababa |
| Ethiopia      | Africa    |  1962|  40.05900|    25145372|    419.4564| Addis Ababa |
| Ethiopia      | Africa    |  1967|  42.11500|    27860297|    516.1186| Addis Ababa |
| Ethiopia      | Africa    |  1972|  43.51500|    30770372|    566.2439| Addis Ababa |
| Ethiopia      | Africa    |  1977|  44.51000|    34617799|    556.8084| Addis Ababa |
| Ethiopia      | Africa    |  1982|  44.91600|    38111756|    577.8607| Addis Ababa |
| Ethiopia      | Africa    |  1987|  46.68400|    42999530|    573.7413| Addis Ababa |
| Ethiopia      | Africa    |  1992|  48.09100|    52088559|    421.3535| Addis Ababa |
| Ethiopia      | Africa    |  1997|  49.40200|    59861301|    515.8894| Addis Ababa |
| Ethiopia      | Africa    |  2002|  50.72500|    67946797|    530.0535| Addis Ababa |
| Ethiopia      | Africa    |  2007|  52.94700|    76511887|    690.8056| Addis Ababa |
| France        | Europe    |  1952|  67.41000|    42459667|   7029.8093| Paris       |
| France        | Europe    |  1957|  68.93000|    44310863|   8662.8349| Paris       |
| France        | Europe    |  1962|  70.51000|    47124000|  10560.4855| Paris       |
| France        | Europe    |  1967|  71.55000|    49569000|  12999.9177| Paris       |
| France        | Europe    |  1972|  72.38000|    51732000|  16107.1917| Paris       |
| France        | Europe    |  1977|  73.83000|    53165019|  18292.6351| Paris       |
| France        | Europe    |  1982|  74.89000|    54433565|  20293.8975| Paris       |
| France        | Europe    |  1987|  76.34000|    55630100|  22066.4421| Paris       |
| France        | Europe    |  1992|  77.46000|    57374179|  24703.7961| Paris       |
| France        | Europe    |  1997|  78.64000|    58623428|  25889.7849| Paris       |
| France        | Europe    |  2002|  79.59000|    59925035|  28926.0323| Paris       |
| France        | Europe    |  2007|  80.65700|    61083916|  30470.0167| Paris       |
| Greece        | Europe    |  1952|  65.86000|     7733250|   3530.6901| Athens      |
| Greece        | Europe    |  1957|  67.86000|     8096218|   4916.2999| Athens      |
| Greece        | Europe    |  1962|  69.51000|     8448233|   6017.1907| Athens      |
| Greece        | Europe    |  1967|  71.00000|     8716441|   8513.0970| Athens      |
| Greece        | Europe    |  1972|  72.34000|     8888628|  12724.8296| Athens      |
| Greece        | Europe    |  1977|  73.68000|     9308479|  14195.5243| Athens      |
| Greece        | Europe    |  1982|  75.24000|     9786480|  15268.4209| Athens      |
| Greece        | Europe    |  1987|  76.67000|     9974490|  16120.5284| Athens      |
| Greece        | Europe    |  1992|  77.03000|    10325429|  17541.4963| Athens      |
| Greece        | Europe    |  1997|  77.86900|    10502372|  18747.6981| Athens      |
| Greece        | Europe    |  2002|  78.25600|    10603863|  22514.2548| Athens      |
| Greece        | Europe    |  2007|  79.48300|    10706290|  27538.4119| Athens      |
| Japan         | Asia      |  1952|  63.03000|    86459025|   3216.9563| Tokyo       |
| Japan         | Asia      |  1957|  65.50000|    91563009|   4317.6944| Tokyo       |
| Japan         | Asia      |  1962|  68.73000|    95831757|   6576.6495| Tokyo       |
| Japan         | Asia      |  1967|  71.43000|   100825279|   9847.7886| Tokyo       |
| Japan         | Asia      |  1972|  73.42000|   107188273|  14778.7864| Tokyo       |
| Japan         | Asia      |  1977|  75.38000|   113872473|  16610.3770| Tokyo       |
| Japan         | Asia      |  1982|  77.11000|   118454974|  19384.1057| Tokyo       |
| Japan         | Asia      |  1987|  78.67000|   122091325|  22375.9419| Tokyo       |
| Japan         | Asia      |  1992|  79.36000|   124329269|  26824.8951| Tokyo       |
| Japan         | Asia      |  1997|  80.69000|   125956499|  28816.5850| Tokyo       |
| Japan         | Asia      |  2002|  82.00000|   127065841|  28604.5919| Tokyo       |
| Japan         | Asia      |  2007|  82.60300|   127467972|  31656.0681| Tokyo       |
| Mexico        | Americas  |  1952|  50.78900|    30144317|   3478.1255| Mexico City |
| Mexico        | Americas  |  1957|  55.19000|    35015548|   4131.5466| Mexico City |
| Mexico        | Americas  |  1962|  58.29900|    41121485|   4581.6094| Mexico City |
| Mexico        | Americas  |  1967|  60.11000|    47995559|   5754.7339| Mexico City |
| Mexico        | Americas  |  1972|  62.36100|    55984294|   6809.4067| Mexico City |
| Mexico        | Americas  |  1977|  65.03200|    63759976|   7674.9291| Mexico City |
| Mexico        | Americas  |  1982|  67.40500|    71640904|   9611.1475| Mexico City |
| Mexico        | Americas  |  1987|  69.49800|    80122492|   8688.1560| Mexico City |
| Mexico        | Americas  |  1992|  71.45500|    88111030|   9472.3843| Mexico City |
| Mexico        | Americas  |  1997|  73.67000|    95895146|   9767.2975| Mexico City |
| Mexico        | Americas  |  2002|  74.90200|   102479927|  10742.4405| Mexico City |
| Mexico        | Americas  |  2007|  76.19500|   108700891|  11977.5750| Mexico City |
| New Zealand   | Oceania   |  1952|  69.39000|     1994794|  10556.5757| Wellington  |
| New Zealand   | Oceania   |  1957|  70.26000|     2229407|  12247.3953| Wellington  |
| New Zealand   | Oceania   |  1962|  71.24000|     2488550|  13175.6780| Wellington  |
| New Zealand   | Oceania   |  1967|  71.52000|     2728150|  14463.9189| Wellington  |
| New Zealand   | Oceania   |  1972|  71.89000|     2929100|  16046.0373| Wellington  |
| New Zealand   | Oceania   |  1977|  72.22000|     3164900|  16233.7177| Wellington  |
| New Zealand   | Oceania   |  1982|  73.84000|     3210650|  17632.4104| Wellington  |
| New Zealand   | Oceania   |  1987|  74.32000|     3317166|  19007.1913| Wellington  |
| New Zealand   | Oceania   |  1992|  76.33000|     3437674|  18363.3249| Wellington  |
| New Zealand   | Oceania   |  1997|  77.55000|     3676187|  21050.4138| Wellington  |
| New Zealand   | Oceania   |  2002|  79.11000|     3908037|  23189.8014| Wellington  |
| New Zealand   | Oceania   |  2007|  80.20400|     4115771|  25185.0091| Wellington  |
| Norway        | Europe    |  1952|  72.67000|     3327728|  10095.4217| Oslo        |
| Norway        | Europe    |  1957|  73.44000|     3491938|  11653.9730| Oslo        |
| Norway        | Europe    |  1962|  73.47000|     3638919|  13450.4015| Oslo        |
| Norway        | Europe    |  1967|  74.08000|     3786019|  16361.8765| Oslo        |
| Norway        | Europe    |  1972|  74.34000|     3933004|  18965.0555| Oslo        |
| Norway        | Europe    |  1977|  75.37000|     4043205|  23311.3494| Oslo        |
| Norway        | Europe    |  1982|  75.97000|     4114787|  26298.6353| Oslo        |
| Norway        | Europe    |  1987|  75.89000|     4186147|  31540.9748| Oslo        |
| Norway        | Europe    |  1992|  77.32000|     4286357|  33965.6611| Oslo        |
| Norway        | Europe    |  1997|  78.32000|     4405672|  41283.1643| Oslo        |
| Norway        | Europe    |  2002|  79.05000|     4535591|  44683.9753| Oslo        |
| Norway        | Europe    |  2007|  80.19600|     4627926|  49357.1902| Oslo        |
| Somalia       | Africa    |  1952|  32.97800|     2526994|   1135.7498| Mogadishu   |
| Somalia       | Africa    |  1957|  34.97700|     2780415|   1258.1474| Mogadishu   |
| Somalia       | Africa    |  1962|  36.98100|     3080153|   1369.4883| Mogadishu   |
| Somalia       | Africa    |  1967|  38.97700|     3428839|   1284.7332| Mogadishu   |
| Somalia       | Africa    |  1972|  40.97300|     3840161|   1254.5761| Mogadishu   |
| Somalia       | Africa    |  1977|  41.97400|     4353666|   1450.9925| Mogadishu   |
| Somalia       | Africa    |  1982|  42.95500|     5828892|   1176.8070| Mogadishu   |
| Somalia       | Africa    |  1987|  44.50100|     6921858|   1093.2450| Mogadishu   |
| Somalia       | Africa    |  1992|  39.65800|     6099799|    926.9603| Mogadishu   |
| Somalia       | Africa    |  1997|  43.79500|     6633514|    930.5964| Mogadishu   |
| Somalia       | Africa    |  2002|  45.93600|     7753310|    882.0818| Mogadishu   |
| Somalia       | Africa    |  2007|  48.15900|     9118773|    926.1411| Mogadishu   |
| Thailand      | Asia      |  1952|  50.84800|    21289402|    757.7974| Bangkok     |
| Thailand      | Asia      |  1957|  53.63000|    25041917|    793.5774| Bangkok     |
| Thailand      | Asia      |  1962|  56.06100|    29263397|   1002.1992| Bangkok     |
| Thailand      | Asia      |  1967|  58.28500|    34024249|   1295.4607| Bangkok     |
| Thailand      | Asia      |  1972|  60.40500|    39276153|   1524.3589| Bangkok     |
| Thailand      | Asia      |  1977|  62.49400|    44148285|   1961.2246| Bangkok     |
| Thailand      | Asia      |  1982|  64.59700|    48827160|   2393.2198| Bangkok     |
| Thailand      | Asia      |  1987|  66.08400|    52910342|   2982.6538| Bangkok     |
| Thailand      | Asia      |  1992|  67.29800|    56667095|   4616.8965| Bangkok     |
| Thailand      | Asia      |  1997|  67.52100|    60216677|   5852.6255| Bangkok     |
| Thailand      | Asia      |  2002|  68.56400|    62806748|   5913.1875| Bangkok     |
| Thailand      | Asia      |  2007|  70.61600|    65068149|   7458.3963| Bangkok     |
| Turkey        | Europe    |  1952|  43.58500|    22235677|   1969.1010| Ankara      |
| Turkey        | Europe    |  1957|  48.07900|    25670939|   2218.7543| Ankara      |
| Turkey        | Europe    |  1962|  52.09800|    29788695|   2322.8699| Ankara      |
| Turkey        | Europe    |  1967|  54.33600|    33411317|   2826.3564| Ankara      |
| Turkey        | Europe    |  1972|  57.00500|    37492953|   3450.6964| Ankara      |
| Turkey        | Europe    |  1977|  59.50700|    42404033|   4269.1223| Ankara      |
| Turkey        | Europe    |  1982|  61.03600|    47328791|   4241.3563| Ankara      |
| Turkey        | Europe    |  1987|  63.10800|    52881328|   5089.0437| Ankara      |
| Turkey        | Europe    |  1992|  66.14600|    58179144|   5678.3483| Ankara      |
| Turkey        | Europe    |  1997|  68.83500|    63047647|   6601.4299| Ankara      |
| Turkey        | Europe    |  2002|  70.84500|    67308928|   6508.0857| Ankara      |
| Turkey        | Europe    |  2007|  71.77700|    71158647|   8458.2764| Ankara      |
| United States | Americas  |  1952|  68.44000|   157553000|  13990.4821| Washington  |
| United States | Americas  |  1957|  69.49000|   171984000|  14847.1271| Washington  |
| United States | Americas  |  1962|  70.21000|   186538000|  16173.1459| Washington  |
| United States | Americas  |  1967|  70.76000|   198712000|  19530.3656| Washington  |
| United States | Americas  |  1972|  71.34000|   209896000|  21806.0359| Washington  |
| United States | Americas  |  1977|  73.38000|   220239000|  24072.6321| Washington  |
| United States | Americas  |  1982|  74.65000|   232187835|  25009.5591| Washington  |
| United States | Americas  |  1987|  75.02000|   242803533|  29884.3504| Washington  |
| United States | Americas  |  1992|  76.09000|   256894189|  32003.9322| Washington  |
| United States | Americas  |  1997|  76.81000|   272911760|  35767.4330| Washington  |
| United States | Americas  |  2002|  77.31000|   287675526|  39097.0995| Washington  |
| United States | Americas  |  2007|  78.24200|   301139947|  42951.6531| Washington  |
| Uruguay       | Americas  |  1952|  66.07100|     2252965|   5716.7667| Montevideo  |
| Uruguay       | Americas  |  1957|  67.04400|     2424959|   6150.7730| Montevideo  |
| Uruguay       | Americas  |  1962|  68.25300|     2598466|   5603.3577| Montevideo  |
| Uruguay       | Americas  |  1967|  68.46800|     2748579|   5444.6196| Montevideo  |
| Uruguay       | Americas  |  1972|  68.67300|     2829526|   5703.4089| Montevideo  |
| Uruguay       | Americas  |  1977|  69.48100|     2873520|   6504.3397| Montevideo  |
| Uruguay       | Americas  |  1982|  70.80500|     2953997|   6920.2231| Montevideo  |
| Uruguay       | Americas  |  1987|  71.91800|     3045153|   7452.3990| Montevideo  |
| Uruguay       | Americas  |  1992|  72.75200|     3149262|   8137.0048| Montevideo  |
| Uruguay       | Americas  |  1997|  74.22300|     3262838|   9230.2407| Montevideo  |
| Uruguay       | Americas  |  2002|  75.30700|     3363085|   7727.0020| Montevideo  |
| Uruguay       | Americas  |  2007|  76.38400|     3447496|  10611.4630| Montevideo  |
| Zimbabwe      | Africa    |  1952|  48.45100|     3080907|    406.8841| Harare      |
| Zimbabwe      | Africa    |  1957|  50.46900|     3646340|    518.7643| Harare      |
| Zimbabwe      | Africa    |  1962|  52.35800|     4277736|    527.2722| Harare      |
| Zimbabwe      | Africa    |  1967|  53.99500|     4995432|    569.7951| Harare      |
| Zimbabwe      | Africa    |  1972|  55.63500|     5861135|    799.3622| Harare      |
| Zimbabwe      | Africa    |  1977|  57.67400|     6642107|    685.5877| Harare      |
| Zimbabwe      | Africa    |  1982|  60.36300|     7636524|    788.8550| Harare      |
| Zimbabwe      | Africa    |  1987|  62.35100|     9216418|    706.1573| Harare      |
| Zimbabwe      | Africa    |  1992|  60.37700|    10704340|    693.4208| Harare      |
| Zimbabwe      | Africa    |  1997|  46.80900|    11404948|    792.4500| Harare      |
| Zimbabwe      | Africa    |  2002|  39.98900|    11926563|    672.0386| Harare      |
| Zimbabwe      | Africa    |  2007|  43.48700|    12311143|    469.7093| Harare      |

`inner_join()` function keeps the rows that are present in both data frames. In this case, it happens using the column `country` (same column name in both tibbles). The resulting dataset consists of 7 columns (6 from gapminder and one from countries tibble). There are 204 rows because each of the 17 countries has data from 12 different years. One country (Russia) from countries\_18 is not present in gapminder so it is removed.

``` r
joined_left <- left_join(gapminder, countries_18)
```

    ## Joining, by = "country"

    ## Warning: Column `country` joining factor and character vector, coercing
    ## into character vector

``` r
str(joined_left)
```

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    1704 obs. of  7 variables:
    ##  $ country  : chr  "Afghanistan" "Afghanistan" "Afghanistan" "Afghanistan" ...
    ##  $ continent: Factor w/ 5 levels "Africa","Americas",..: 3 3 3 3 3 3 3 3 3 3 ...
    ##  $ year     : int  1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
    ##  $ lifeExp  : num  28.8 30.3 32 34 36.1 ...
    ##  $ pop      : int  8425333 9240934 10267083 11537966 13079460 14880372 12881816 13867957 16317921 22227415 ...
    ##  $ gdpPercap: num  779 821 853 836 740 ...
    ##  $ capital  : chr  NA NA NA NA ...

``` r
levels(as.factor(joined_left$country))
```

    ##   [1] "Afghanistan"              "Albania"                 
    ##   [3] "Algeria"                  "Angola"                  
    ##   [5] "Argentina"                "Australia"               
    ##   [7] "Austria"                  "Bahrain"                 
    ##   [9] "Bangladesh"               "Belgium"                 
    ##  [11] "Benin"                    "Bolivia"                 
    ##  [13] "Bosnia and Herzegovina"   "Botswana"                
    ##  [15] "Brazil"                   "Bulgaria"                
    ##  [17] "Burkina Faso"             "Burundi"                 
    ##  [19] "Cambodia"                 "Cameroon"                
    ##  [21] "Canada"                   "Central African Republic"
    ##  [23] "Chad"                     "Chile"                   
    ##  [25] "China"                    "Colombia"                
    ##  [27] "Comoros"                  "Congo, Dem. Rep."        
    ##  [29] "Congo, Rep."              "Costa Rica"              
    ##  [31] "Cote d'Ivoire"            "Croatia"                 
    ##  [33] "Cuba"                     "Czech Republic"          
    ##  [35] "Denmark"                  "Djibouti"                
    ##  [37] "Dominican Republic"       "Ecuador"                 
    ##  [39] "Egypt"                    "El Salvador"             
    ##  [41] "Equatorial Guinea"        "Eritrea"                 
    ##  [43] "Ethiopia"                 "Finland"                 
    ##  [45] "France"                   "Gabon"                   
    ##  [47] "Gambia"                   "Germany"                 
    ##  [49] "Ghana"                    "Greece"                  
    ##  [51] "Guatemala"                "Guinea"                  
    ##  [53] "Guinea-Bissau"            "Haiti"                   
    ##  [55] "Honduras"                 "Hong Kong, China"        
    ##  [57] "Hungary"                  "Iceland"                 
    ##  [59] "India"                    "Indonesia"               
    ##  [61] "Iran"                     "Iraq"                    
    ##  [63] "Ireland"                  "Israel"                  
    ##  [65] "Italy"                    "Jamaica"                 
    ##  [67] "Japan"                    "Jordan"                  
    ##  [69] "Kenya"                    "Korea, Dem. Rep."        
    ##  [71] "Korea, Rep."              "Kuwait"                  
    ##  [73] "Lebanon"                  "Lesotho"                 
    ##  [75] "Liberia"                  "Libya"                   
    ##  [77] "Madagascar"               "Malawi"                  
    ##  [79] "Malaysia"                 "Mali"                    
    ##  [81] "Mauritania"               "Mauritius"               
    ##  [83] "Mexico"                   "Mongolia"                
    ##  [85] "Montenegro"               "Morocco"                 
    ##  [87] "Mozambique"               "Myanmar"                 
    ##  [89] "Namibia"                  "Nepal"                   
    ##  [91] "Netherlands"              "New Zealand"             
    ##  [93] "Nicaragua"                "Niger"                   
    ##  [95] "Nigeria"                  "Norway"                  
    ##  [97] "Oman"                     "Pakistan"                
    ##  [99] "Panama"                   "Paraguay"                
    ## [101] "Peru"                     "Philippines"             
    ## [103] "Poland"                   "Portugal"                
    ## [105] "Puerto Rico"              "Reunion"                 
    ## [107] "Romania"                  "Rwanda"                  
    ## [109] "Sao Tome and Principe"    "Saudi Arabia"            
    ## [111] "Senegal"                  "Serbia"                  
    ## [113] "Sierra Leone"             "Singapore"               
    ## [115] "Slovak Republic"          "Slovenia"                
    ## [117] "Somalia"                  "South Africa"            
    ## [119] "Spain"                    "Sri Lanka"               
    ## [121] "Sudan"                    "Swaziland"               
    ## [123] "Sweden"                   "Switzerland"             
    ## [125] "Syria"                    "Taiwan"                  
    ## [127] "Tanzania"                 "Thailand"                
    ## [129] "Togo"                     "Trinidad and Tobago"     
    ## [131] "Tunisia"                  "Turkey"                  
    ## [133] "Uganda"                   "United Kingdom"          
    ## [135] "United States"            "Uruguay"                 
    ## [137] "Venezuela"                "Vietnam"                 
    ## [139] "West Bank and Gaza"       "Yemen, Rep."             
    ## [141] "Zambia"                   "Zimbabwe"

``` r
levels(as.factor(joined_left$year))
```

    ##  [1] "1952" "1957" "1962" "1967" "1972" "1977" "1982" "1987" "1992" "1997"
    ## [11] "2002" "2007"

``` r
knitr::kable(joined_left)
```

| country                  | continent |  year|   lifeExp|         pop|    gdpPercap| capital     |
|:-------------------------|:----------|-----:|---------:|-----------:|------------:|:------------|
| Afghanistan              | Asia      |  1952|  28.80100|     8425333|     779.4453| NA          |
| Afghanistan              | Asia      |  1957|  30.33200|     9240934|     820.8530| NA          |
| Afghanistan              | Asia      |  1962|  31.99700|    10267083|     853.1007| NA          |
| Afghanistan              | Asia      |  1967|  34.02000|    11537966|     836.1971| NA          |
| Afghanistan              | Asia      |  1972|  36.08800|    13079460|     739.9811| NA          |
| Afghanistan              | Asia      |  1977|  38.43800|    14880372|     786.1134| NA          |
| Afghanistan              | Asia      |  1982|  39.85400|    12881816|     978.0114| NA          |
| Afghanistan              | Asia      |  1987|  40.82200|    13867957|     852.3959| NA          |
| Afghanistan              | Asia      |  1992|  41.67400|    16317921|     649.3414| NA          |
| Afghanistan              | Asia      |  1997|  41.76300|    22227415|     635.3414| NA          |
| Afghanistan              | Asia      |  2002|  42.12900|    25268405|     726.7341| NA          |
| Afghanistan              | Asia      |  2007|  43.82800|    31889923|     974.5803| NA          |
| Albania                  | Europe    |  1952|  55.23000|     1282697|    1601.0561| NA          |
| Albania                  | Europe    |  1957|  59.28000|     1476505|    1942.2842| NA          |
| Albania                  | Europe    |  1962|  64.82000|     1728137|    2312.8890| NA          |
| Albania                  | Europe    |  1967|  66.22000|     1984060|    2760.1969| NA          |
| Albania                  | Europe    |  1972|  67.69000|     2263554|    3313.4222| NA          |
| Albania                  | Europe    |  1977|  68.93000|     2509048|    3533.0039| NA          |
| Albania                  | Europe    |  1982|  70.42000|     2780097|    3630.8807| NA          |
| Albania                  | Europe    |  1987|  72.00000|     3075321|    3738.9327| NA          |
| Albania                  | Europe    |  1992|  71.58100|     3326498|    2497.4379| NA          |
| Albania                  | Europe    |  1997|  72.95000|     3428038|    3193.0546| NA          |
| Albania                  | Europe    |  2002|  75.65100|     3508512|    4604.2117| NA          |
| Albania                  | Europe    |  2007|  76.42300|     3600523|    5937.0295| NA          |
| Algeria                  | Africa    |  1952|  43.07700|     9279525|    2449.0082| NA          |
| Algeria                  | Africa    |  1957|  45.68500|    10270856|    3013.9760| NA          |
| Algeria                  | Africa    |  1962|  48.30300|    11000948|    2550.8169| NA          |
| Algeria                  | Africa    |  1967|  51.40700|    12760499|    3246.9918| NA          |
| Algeria                  | Africa    |  1972|  54.51800|    14760787|    4182.6638| NA          |
| Algeria                  | Africa    |  1977|  58.01400|    17152804|    4910.4168| NA          |
| Algeria                  | Africa    |  1982|  61.36800|    20033753|    5745.1602| NA          |
| Algeria                  | Africa    |  1987|  65.79900|    23254956|    5681.3585| NA          |
| Algeria                  | Africa    |  1992|  67.74400|    26298373|    5023.2166| NA          |
| Algeria                  | Africa    |  1997|  69.15200|    29072015|    4797.2951| NA          |
| Algeria                  | Africa    |  2002|  70.99400|    31287142|    5288.0404| NA          |
| Algeria                  | Africa    |  2007|  72.30100|    33333216|    6223.3675| NA          |
| Angola                   | Africa    |  1952|  30.01500|     4232095|    3520.6103| NA          |
| Angola                   | Africa    |  1957|  31.99900|     4561361|    3827.9405| NA          |
| Angola                   | Africa    |  1962|  34.00000|     4826015|    4269.2767| NA          |
| Angola                   | Africa    |  1967|  35.98500|     5247469|    5522.7764| NA          |
| Angola                   | Africa    |  1972|  37.92800|     5894858|    5473.2880| NA          |
| Angola                   | Africa    |  1977|  39.48300|     6162675|    3008.6474| NA          |
| Angola                   | Africa    |  1982|  39.94200|     7016384|    2756.9537| NA          |
| Angola                   | Africa    |  1987|  39.90600|     7874230|    2430.2083| NA          |
| Angola                   | Africa    |  1992|  40.64700|     8735988|    2627.8457| NA          |
| Angola                   | Africa    |  1997|  40.96300|     9875024|    2277.1409| NA          |
| Angola                   | Africa    |  2002|  41.00300|    10866106|    2773.2873| NA          |
| Angola                   | Africa    |  2007|  42.73100|    12420476|    4797.2313| NA          |
| Argentina                | Americas  |  1952|  62.48500|    17876956|    5911.3151| NA          |
| Argentina                | Americas  |  1957|  64.39900|    19610538|    6856.8562| NA          |
| Argentina                | Americas  |  1962|  65.14200|    21283783|    7133.1660| NA          |
| Argentina                | Americas  |  1967|  65.63400|    22934225|    8052.9530| NA          |
| Argentina                | Americas  |  1972|  67.06500|    24779799|    9443.0385| NA          |
| Argentina                | Americas  |  1977|  68.48100|    26983828|   10079.0267| NA          |
| Argentina                | Americas  |  1982|  69.94200|    29341374|    8997.8974| NA          |
| Argentina                | Americas  |  1987|  70.77400|    31620918|    9139.6714| NA          |
| Argentina                | Americas  |  1992|  71.86800|    33958947|    9308.4187| NA          |
| Argentina                | Americas  |  1997|  73.27500|    36203463|   10967.2820| NA          |
| Argentina                | Americas  |  2002|  74.34000|    38331121|    8797.6407| NA          |
| Argentina                | Americas  |  2007|  75.32000|    40301927|   12779.3796| NA          |
| Australia                | Oceania   |  1952|  69.12000|     8691212|   10039.5956| Canberra    |
| Australia                | Oceania   |  1957|  70.33000|     9712569|   10949.6496| Canberra    |
| Australia                | Oceania   |  1962|  70.93000|    10794968|   12217.2269| Canberra    |
| Australia                | Oceania   |  1967|  71.10000|    11872264|   14526.1246| Canberra    |
| Australia                | Oceania   |  1972|  71.93000|    13177000|   16788.6295| Canberra    |
| Australia                | Oceania   |  1977|  73.49000|    14074100|   18334.1975| Canberra    |
| Australia                | Oceania   |  1982|  74.74000|    15184200|   19477.0093| Canberra    |
| Australia                | Oceania   |  1987|  76.32000|    16257249|   21888.8890| Canberra    |
| Australia                | Oceania   |  1992|  77.56000|    17481977|   23424.7668| Canberra    |
| Australia                | Oceania   |  1997|  78.83000|    18565243|   26997.9366| Canberra    |
| Australia                | Oceania   |  2002|  80.37000|    19546792|   30687.7547| Canberra    |
| Australia                | Oceania   |  2007|  81.23500|    20434176|   34435.3674| Canberra    |
| Austria                  | Europe    |  1952|  66.80000|     6927772|    6137.0765| NA          |
| Austria                  | Europe    |  1957|  67.48000|     6965860|    8842.5980| NA          |
| Austria                  | Europe    |  1962|  69.54000|     7129864|   10750.7211| NA          |
| Austria                  | Europe    |  1967|  70.14000|     7376998|   12834.6024| NA          |
| Austria                  | Europe    |  1972|  70.63000|     7544201|   16661.6256| NA          |
| Austria                  | Europe    |  1977|  72.17000|     7568430|   19749.4223| NA          |
| Austria                  | Europe    |  1982|  73.18000|     7574613|   21597.0836| NA          |
| Austria                  | Europe    |  1987|  74.94000|     7578903|   23687.8261| NA          |
| Austria                  | Europe    |  1992|  76.04000|     7914969|   27042.0187| NA          |
| Austria                  | Europe    |  1997|  77.51000|     8069876|   29095.9207| NA          |
| Austria                  | Europe    |  2002|  78.98000|     8148312|   32417.6077| NA          |
| Austria                  | Europe    |  2007|  79.82900|     8199783|   36126.4927| NA          |
| Bahrain                  | Asia      |  1952|  50.93900|      120447|    9867.0848| NA          |
| Bahrain                  | Asia      |  1957|  53.83200|      138655|   11635.7995| NA          |
| Bahrain                  | Asia      |  1962|  56.92300|      171863|   12753.2751| NA          |
| Bahrain                  | Asia      |  1967|  59.92300|      202182|   14804.6727| NA          |
| Bahrain                  | Asia      |  1972|  63.30000|      230800|   18268.6584| NA          |
| Bahrain                  | Asia      |  1977|  65.59300|      297410|   19340.1020| NA          |
| Bahrain                  | Asia      |  1982|  69.05200|      377967|   19211.1473| NA          |
| Bahrain                  | Asia      |  1987|  70.75000|      454612|   18524.0241| NA          |
| Bahrain                  | Asia      |  1992|  72.60100|      529491|   19035.5792| NA          |
| Bahrain                  | Asia      |  1997|  73.92500|      598561|   20292.0168| NA          |
| Bahrain                  | Asia      |  2002|  74.79500|      656397|   23403.5593| NA          |
| Bahrain                  | Asia      |  2007|  75.63500|      708573|   29796.0483| NA          |
| Bangladesh               | Asia      |  1952|  37.48400|    46886859|     684.2442| NA          |
| Bangladesh               | Asia      |  1957|  39.34800|    51365468|     661.6375| NA          |
| Bangladesh               | Asia      |  1962|  41.21600|    56839289|     686.3416| NA          |
| Bangladesh               | Asia      |  1967|  43.45300|    62821884|     721.1861| NA          |
| Bangladesh               | Asia      |  1972|  45.25200|    70759295|     630.2336| NA          |
| Bangladesh               | Asia      |  1977|  46.92300|    80428306|     659.8772| NA          |
| Bangladesh               | Asia      |  1982|  50.00900|    93074406|     676.9819| NA          |
| Bangladesh               | Asia      |  1987|  52.81900|   103764241|     751.9794| NA          |
| Bangladesh               | Asia      |  1992|  56.01800|   113704579|     837.8102| NA          |
| Bangladesh               | Asia      |  1997|  59.41200|   123315288|     972.7700| NA          |
| Bangladesh               | Asia      |  2002|  62.01300|   135656790|    1136.3904| NA          |
| Bangladesh               | Asia      |  2007|  64.06200|   150448339|    1391.2538| NA          |
| Belgium                  | Europe    |  1952|  68.00000|     8730405|    8343.1051| NA          |
| Belgium                  | Europe    |  1957|  69.24000|     8989111|    9714.9606| NA          |
| Belgium                  | Europe    |  1962|  70.25000|     9218400|   10991.2068| NA          |
| Belgium                  | Europe    |  1967|  70.94000|     9556500|   13149.0412| NA          |
| Belgium                  | Europe    |  1972|  71.44000|     9709100|   16672.1436| NA          |
| Belgium                  | Europe    |  1977|  72.80000|     9821800|   19117.9745| NA          |
| Belgium                  | Europe    |  1982|  73.93000|     9856303|   20979.8459| NA          |
| Belgium                  | Europe    |  1987|  75.35000|     9870200|   22525.5631| NA          |
| Belgium                  | Europe    |  1992|  76.46000|    10045622|   25575.5707| NA          |
| Belgium                  | Europe    |  1997|  77.53000|    10199787|   27561.1966| NA          |
| Belgium                  | Europe    |  2002|  78.32000|    10311970|   30485.8838| NA          |
| Belgium                  | Europe    |  2007|  79.44100|    10392226|   33692.6051| NA          |
| Benin                    | Africa    |  1952|  38.22300|     1738315|    1062.7522| NA          |
| Benin                    | Africa    |  1957|  40.35800|     1925173|     959.6011| NA          |
| Benin                    | Africa    |  1962|  42.61800|     2151895|     949.4991| NA          |
| Benin                    | Africa    |  1967|  44.88500|     2427334|    1035.8314| NA          |
| Benin                    | Africa    |  1972|  47.01400|     2761407|    1085.7969| NA          |
| Benin                    | Africa    |  1977|  49.19000|     3168267|    1029.1613| NA          |
| Benin                    | Africa    |  1982|  50.90400|     3641603|    1277.8976| NA          |
| Benin                    | Africa    |  1987|  52.33700|     4243788|    1225.8560| NA          |
| Benin                    | Africa    |  1992|  53.91900|     4981671|    1191.2077| NA          |
| Benin                    | Africa    |  1997|  54.77700|     6066080|    1232.9753| NA          |
| Benin                    | Africa    |  2002|  54.40600|     7026113|    1372.8779| NA          |
| Benin                    | Africa    |  2007|  56.72800|     8078314|    1441.2849| NA          |
| Bolivia                  | Americas  |  1952|  40.41400|     2883315|    2677.3263| NA          |
| Bolivia                  | Americas  |  1957|  41.89000|     3211738|    2127.6863| NA          |
| Bolivia                  | Americas  |  1962|  43.42800|     3593918|    2180.9725| NA          |
| Bolivia                  | Americas  |  1967|  45.03200|     4040665|    2586.8861| NA          |
| Bolivia                  | Americas  |  1972|  46.71400|     4565872|    2980.3313| NA          |
| Bolivia                  | Americas  |  1977|  50.02300|     5079716|    3548.0978| NA          |
| Bolivia                  | Americas  |  1982|  53.85900|     5642224|    3156.5105| NA          |
| Bolivia                  | Americas  |  1987|  57.25100|     6156369|    2753.6915| NA          |
| Bolivia                  | Americas  |  1992|  59.95700|     6893451|    2961.6997| NA          |
| Bolivia                  | Americas  |  1997|  62.05000|     7693188|    3326.1432| NA          |
| Bolivia                  | Americas  |  2002|  63.88300|     8445134|    3413.2627| NA          |
| Bolivia                  | Americas  |  2007|  65.55400|     9119152|    3822.1371| NA          |
| Bosnia and Herzegovina   | Europe    |  1952|  53.82000|     2791000|     973.5332| NA          |
| Bosnia and Herzegovina   | Europe    |  1957|  58.45000|     3076000|    1353.9892| NA          |
| Bosnia and Herzegovina   | Europe    |  1962|  61.93000|     3349000|    1709.6837| NA          |
| Bosnia and Herzegovina   | Europe    |  1967|  64.79000|     3585000|    2172.3524| NA          |
| Bosnia and Herzegovina   | Europe    |  1972|  67.45000|     3819000|    2860.1698| NA          |
| Bosnia and Herzegovina   | Europe    |  1977|  69.86000|     4086000|    3528.4813| NA          |
| Bosnia and Herzegovina   | Europe    |  1982|  70.69000|     4172693|    4126.6132| NA          |
| Bosnia and Herzegovina   | Europe    |  1987|  71.14000|     4338977|    4314.1148| NA          |
| Bosnia and Herzegovina   | Europe    |  1992|  72.17800|     4256013|    2546.7814| NA          |
| Bosnia and Herzegovina   | Europe    |  1997|  73.24400|     3607000|    4766.3559| NA          |
| Bosnia and Herzegovina   | Europe    |  2002|  74.09000|     4165416|    6018.9752| NA          |
| Bosnia and Herzegovina   | Europe    |  2007|  74.85200|     4552198|    7446.2988| NA          |
| Botswana                 | Africa    |  1952|  47.62200|      442308|     851.2411| NA          |
| Botswana                 | Africa    |  1957|  49.61800|      474639|     918.2325| NA          |
| Botswana                 | Africa    |  1962|  51.52000|      512764|     983.6540| NA          |
| Botswana                 | Africa    |  1967|  53.29800|      553541|    1214.7093| NA          |
| Botswana                 | Africa    |  1972|  56.02400|      619351|    2263.6111| NA          |
| Botswana                 | Africa    |  1977|  59.31900|      781472|    3214.8578| NA          |
| Botswana                 | Africa    |  1982|  61.48400|      970347|    4551.1421| NA          |
| Botswana                 | Africa    |  1987|  63.62200|     1151184|    6205.8839| NA          |
| Botswana                 | Africa    |  1992|  62.74500|     1342614|    7954.1116| NA          |
| Botswana                 | Africa    |  1997|  52.55600|     1536536|    8647.1423| NA          |
| Botswana                 | Africa    |  2002|  46.63400|     1630347|   11003.6051| NA          |
| Botswana                 | Africa    |  2007|  50.72800|     1639131|   12569.8518| NA          |
| Brazil                   | Americas  |  1952|  50.91700|    56602560|    2108.9444| Brasilia    |
| Brazil                   | Americas  |  1957|  53.28500|    65551171|    2487.3660| Brasilia    |
| Brazil                   | Americas  |  1962|  55.66500|    76039390|    3336.5858| Brasilia    |
| Brazil                   | Americas  |  1967|  57.63200|    88049823|    3429.8644| Brasilia    |
| Brazil                   | Americas  |  1972|  59.50400|   100840058|    4985.7115| Brasilia    |
| Brazil                   | Americas  |  1977|  61.48900|   114313951|    6660.1187| Brasilia    |
| Brazil                   | Americas  |  1982|  63.33600|   128962939|    7030.8359| Brasilia    |
| Brazil                   | Americas  |  1987|  65.20500|   142938076|    7807.0958| Brasilia    |
| Brazil                   | Americas  |  1992|  67.05700|   155975974|    6950.2830| Brasilia    |
| Brazil                   | Americas  |  1997|  69.38800|   168546719|    7957.9808| Brasilia    |
| Brazil                   | Americas  |  2002|  71.00600|   179914212|    8131.2128| Brasilia    |
| Brazil                   | Americas  |  2007|  72.39000|   190010647|    9065.8008| Brasilia    |
| Bulgaria                 | Europe    |  1952|  59.60000|     7274900|    2444.2866| NA          |
| Bulgaria                 | Europe    |  1957|  66.61000|     7651254|    3008.6707| NA          |
| Bulgaria                 | Europe    |  1962|  69.51000|     8012946|    4254.3378| NA          |
| Bulgaria                 | Europe    |  1967|  70.42000|     8310226|    5577.0028| NA          |
| Bulgaria                 | Europe    |  1972|  70.90000|     8576200|    6597.4944| NA          |
| Bulgaria                 | Europe    |  1977|  70.81000|     8797022|    7612.2404| NA          |
| Bulgaria                 | Europe    |  1982|  71.08000|     8892098|    8224.1916| NA          |
| Bulgaria                 | Europe    |  1987|  71.34000|     8971958|    8239.8548| NA          |
| Bulgaria                 | Europe    |  1992|  71.19000|     8658506|    6302.6234| NA          |
| Bulgaria                 | Europe    |  1997|  70.32000|     8066057|    5970.3888| NA          |
| Bulgaria                 | Europe    |  2002|  72.14000|     7661799|    7696.7777| NA          |
| Bulgaria                 | Europe    |  2007|  73.00500|     7322858|   10680.7928| NA          |
| Burkina Faso             | Africa    |  1952|  31.97500|     4469979|     543.2552| NA          |
| Burkina Faso             | Africa    |  1957|  34.90600|     4713416|     617.1835| NA          |
| Burkina Faso             | Africa    |  1962|  37.81400|     4919632|     722.5120| NA          |
| Burkina Faso             | Africa    |  1967|  40.69700|     5127935|     794.8266| NA          |
| Burkina Faso             | Africa    |  1972|  43.59100|     5433886|     854.7360| NA          |
| Burkina Faso             | Africa    |  1977|  46.13700|     5889574|     743.3870| NA          |
| Burkina Faso             | Africa    |  1982|  48.12200|     6634596|     807.1986| NA          |
| Burkina Faso             | Africa    |  1987|  49.55700|     7586551|     912.0631| NA          |
| Burkina Faso             | Africa    |  1992|  50.26000|     8878303|     931.7528| NA          |
| Burkina Faso             | Africa    |  1997|  50.32400|    10352843|     946.2950| NA          |
| Burkina Faso             | Africa    |  2002|  50.65000|    12251209|    1037.6452| NA          |
| Burkina Faso             | Africa    |  2007|  52.29500|    14326203|    1217.0330| NA          |
| Burundi                  | Africa    |  1952|  39.03100|     2445618|     339.2965| NA          |
| Burundi                  | Africa    |  1957|  40.53300|     2667518|     379.5646| NA          |
| Burundi                  | Africa    |  1962|  42.04500|     2961915|     355.2032| NA          |
| Burundi                  | Africa    |  1967|  43.54800|     3330989|     412.9775| NA          |
| Burundi                  | Africa    |  1972|  44.05700|     3529983|     464.0995| NA          |
| Burundi                  | Africa    |  1977|  45.91000|     3834415|     556.1033| NA          |
| Burundi                  | Africa    |  1982|  47.47100|     4580410|     559.6032| NA          |
| Burundi                  | Africa    |  1987|  48.21100|     5126023|     621.8188| NA          |
| Burundi                  | Africa    |  1992|  44.73600|     5809236|     631.6999| NA          |
| Burundi                  | Africa    |  1997|  45.32600|     6121610|     463.1151| NA          |
| Burundi                  | Africa    |  2002|  47.36000|     7021078|     446.4035| NA          |
| Burundi                  | Africa    |  2007|  49.58000|     8390505|     430.0707| NA          |
| Cambodia                 | Asia      |  1952|  39.41700|     4693836|     368.4693| NA          |
| Cambodia                 | Asia      |  1957|  41.36600|     5322536|     434.0383| NA          |
| Cambodia                 | Asia      |  1962|  43.41500|     6083619|     496.9136| NA          |
| Cambodia                 | Asia      |  1967|  45.41500|     6960067|     523.4323| NA          |
| Cambodia                 | Asia      |  1972|  40.31700|     7450606|     421.6240| NA          |
| Cambodia                 | Asia      |  1977|  31.22000|     6978607|     524.9722| NA          |
| Cambodia                 | Asia      |  1982|  50.95700|     7272485|     624.4755| NA          |
| Cambodia                 | Asia      |  1987|  53.91400|     8371791|     683.8956| NA          |
| Cambodia                 | Asia      |  1992|  55.80300|    10150094|     682.3032| NA          |
| Cambodia                 | Asia      |  1997|  56.53400|    11782962|     734.2852| NA          |
| Cambodia                 | Asia      |  2002|  56.75200|    12926707|     896.2260| NA          |
| Cambodia                 | Asia      |  2007|  59.72300|    14131858|    1713.7787| NA          |
| Cameroon                 | Africa    |  1952|  38.52300|     5009067|    1172.6677| NA          |
| Cameroon                 | Africa    |  1957|  40.42800|     5359923|    1313.0481| NA          |
| Cameroon                 | Africa    |  1962|  42.64300|     5793633|    1399.6074| NA          |
| Cameroon                 | Africa    |  1967|  44.79900|     6335506|    1508.4531| NA          |
| Cameroon                 | Africa    |  1972|  47.04900|     7021028|    1684.1465| NA          |
| Cameroon                 | Africa    |  1977|  49.35500|     7959865|    1783.4329| NA          |
| Cameroon                 | Africa    |  1982|  52.96100|     9250831|    2367.9833| NA          |
| Cameroon                 | Africa    |  1987|  54.98500|    10780667|    2602.6642| NA          |
| Cameroon                 | Africa    |  1992|  54.31400|    12467171|    1793.1633| NA          |
| Cameroon                 | Africa    |  1997|  52.19900|    14195809|    1694.3375| NA          |
| Cameroon                 | Africa    |  2002|  49.85600|    15929988|    1934.0114| NA          |
| Cameroon                 | Africa    |  2007|  50.43000|    17696293|    2042.0952| NA          |
| Canada                   | Americas  |  1952|  68.75000|    14785584|   11367.1611| Ottawa      |
| Canada                   | Americas  |  1957|  69.96000|    17010154|   12489.9501| Ottawa      |
| Canada                   | Americas  |  1962|  71.30000|    18985849|   13462.4855| Ottawa      |
| Canada                   | Americas  |  1967|  72.13000|    20819767|   16076.5880| Ottawa      |
| Canada                   | Americas  |  1972|  72.88000|    22284500|   18970.5709| Ottawa      |
| Canada                   | Americas  |  1977|  74.21000|    23796400|   22090.8831| Ottawa      |
| Canada                   | Americas  |  1982|  75.76000|    25201900|   22898.7921| Ottawa      |
| Canada                   | Americas  |  1987|  76.86000|    26549700|   26626.5150| Ottawa      |
| Canada                   | Americas  |  1992|  77.95000|    28523502|   26342.8843| Ottawa      |
| Canada                   | Americas  |  1997|  78.61000|    30305843|   28954.9259| Ottawa      |
| Canada                   | Americas  |  2002|  79.77000|    31902268|   33328.9651| Ottawa      |
| Canada                   | Americas  |  2007|  80.65300|    33390141|   36319.2350| Ottawa      |
| Central African Republic | Africa    |  1952|  35.46300|     1291695|    1071.3107| NA          |
| Central African Republic | Africa    |  1957|  37.46400|     1392284|    1190.8443| NA          |
| Central African Republic | Africa    |  1962|  39.47500|     1523478|    1193.0688| NA          |
| Central African Republic | Africa    |  1967|  41.47800|     1733638|    1136.0566| NA          |
| Central African Republic | Africa    |  1972|  43.45700|     1927260|    1070.0133| NA          |
| Central African Republic | Africa    |  1977|  46.77500|     2167533|    1109.3743| NA          |
| Central African Republic | Africa    |  1982|  48.29500|     2476971|     956.7530| NA          |
| Central African Republic | Africa    |  1987|  50.48500|     2840009|     844.8764| NA          |
| Central African Republic | Africa    |  1992|  49.39600|     3265124|     747.9055| NA          |
| Central African Republic | Africa    |  1997|  46.06600|     3696513|     740.5063| NA          |
| Central African Republic | Africa    |  2002|  43.30800|     4048013|     738.6906| NA          |
| Central African Republic | Africa    |  2007|  44.74100|     4369038|     706.0165| NA          |
| Chad                     | Africa    |  1952|  38.09200|     2682462|    1178.6659| NA          |
| Chad                     | Africa    |  1957|  39.88100|     2894855|    1308.4956| NA          |
| Chad                     | Africa    |  1962|  41.71600|     3150417|    1389.8176| NA          |
| Chad                     | Africa    |  1967|  43.60100|     3495967|    1196.8106| NA          |
| Chad                     | Africa    |  1972|  45.56900|     3899068|    1104.1040| NA          |
| Chad                     | Africa    |  1977|  47.38300|     4388260|    1133.9850| NA          |
| Chad                     | Africa    |  1982|  49.51700|     4875118|     797.9081| NA          |
| Chad                     | Africa    |  1987|  51.05100|     5498955|     952.3861| NA          |
| Chad                     | Africa    |  1992|  51.72400|     6429417|    1058.0643| NA          |
| Chad                     | Africa    |  1997|  51.57300|     7562011|    1004.9614| NA          |
| Chad                     | Africa    |  2002|  50.52500|     8835739|    1156.1819| NA          |
| Chad                     | Africa    |  2007|  50.65100|    10238807|    1704.0637| NA          |
| Chile                    | Americas  |  1952|  54.74500|     6377619|    3939.9788| NA          |
| Chile                    | Americas  |  1957|  56.07400|     7048426|    4315.6227| NA          |
| Chile                    | Americas  |  1962|  57.92400|     7961258|    4519.0943| NA          |
| Chile                    | Americas  |  1967|  60.52300|     8858908|    5106.6543| NA          |
| Chile                    | Americas  |  1972|  63.44100|     9717524|    5494.0244| NA          |
| Chile                    | Americas  |  1977|  67.05200|    10599793|    4756.7638| NA          |
| Chile                    | Americas  |  1982|  70.56500|    11487112|    5095.6657| NA          |
| Chile                    | Americas  |  1987|  72.49200|    12463354|    5547.0638| NA          |
| Chile                    | Americas  |  1992|  74.12600|    13572994|    7596.1260| NA          |
| Chile                    | Americas  |  1997|  75.81600|    14599929|   10118.0532| NA          |
| Chile                    | Americas  |  2002|  77.86000|    15497046|   10778.7838| NA          |
| Chile                    | Americas  |  2007|  78.55300|    16284741|   13171.6388| NA          |
| China                    | Asia      |  1952|  44.00000|   556263527|     400.4486| Beijing     |
| China                    | Asia      |  1957|  50.54896|   637408000|     575.9870| Beijing     |
| China                    | Asia      |  1962|  44.50136|   665770000|     487.6740| Beijing     |
| China                    | Asia      |  1967|  58.38112|   754550000|     612.7057| Beijing     |
| China                    | Asia      |  1972|  63.11888|   862030000|     676.9001| Beijing     |
| China                    | Asia      |  1977|  63.96736|   943455000|     741.2375| Beijing     |
| China                    | Asia      |  1982|  65.52500|  1000281000|     962.4214| Beijing     |
| China                    | Asia      |  1987|  67.27400|  1084035000|    1378.9040| Beijing     |
| China                    | Asia      |  1992|  68.69000|  1164970000|    1655.7842| Beijing     |
| China                    | Asia      |  1997|  70.42600|  1230075000|    2289.2341| Beijing     |
| China                    | Asia      |  2002|  72.02800|  1280400000|    3119.2809| Beijing     |
| China                    | Asia      |  2007|  72.96100|  1318683096|    4959.1149| Beijing     |
| Colombia                 | Americas  |  1952|  50.64300|    12350771|    2144.1151| NA          |
| Colombia                 | Americas  |  1957|  55.11800|    14485993|    2323.8056| NA          |
| Colombia                 | Americas  |  1962|  57.86300|    17009885|    2492.3511| NA          |
| Colombia                 | Americas  |  1967|  59.96300|    19764027|    2678.7298| NA          |
| Colombia                 | Americas  |  1972|  61.62300|    22542890|    3264.6600| NA          |
| Colombia                 | Americas  |  1977|  63.83700|    25094412|    3815.8079| NA          |
| Colombia                 | Americas  |  1982|  66.65300|    27764644|    4397.5757| NA          |
| Colombia                 | Americas  |  1987|  67.76800|    30964245|    4903.2191| NA          |
| Colombia                 | Americas  |  1992|  68.42100|    34202721|    5444.6486| NA          |
| Colombia                 | Americas  |  1997|  70.31300|    37657830|    6117.3617| NA          |
| Colombia                 | Americas  |  2002|  71.68200|    41008227|    5755.2600| NA          |
| Colombia                 | Americas  |  2007|  72.88900|    44227550|    7006.5804| NA          |
| Comoros                  | Africa    |  1952|  40.71500|      153936|    1102.9909| NA          |
| Comoros                  | Africa    |  1957|  42.46000|      170928|    1211.1485| NA          |
| Comoros                  | Africa    |  1962|  44.46700|      191689|    1406.6483| NA          |
| Comoros                  | Africa    |  1967|  46.47200|      217378|    1876.0296| NA          |
| Comoros                  | Africa    |  1972|  48.94400|      250027|    1937.5777| NA          |
| Comoros                  | Africa    |  1977|  50.93900|      304739|    1172.6030| NA          |
| Comoros                  | Africa    |  1982|  52.93300|      348643|    1267.1001| NA          |
| Comoros                  | Africa    |  1987|  54.92600|      395114|    1315.9808| NA          |
| Comoros                  | Africa    |  1992|  57.93900|      454429|    1246.9074| NA          |
| Comoros                  | Africa    |  1997|  60.66000|      527982|    1173.6182| NA          |
| Comoros                  | Africa    |  2002|  62.97400|      614382|    1075.8116| NA          |
| Comoros                  | Africa    |  2007|  65.15200|      710960|     986.1479| NA          |
| Congo, Dem. Rep.         | Africa    |  1952|  39.14300|    14100005|     780.5423| NA          |
| Congo, Dem. Rep.         | Africa    |  1957|  40.65200|    15577932|     905.8602| NA          |
| Congo, Dem. Rep.         | Africa    |  1962|  42.12200|    17486434|     896.3146| NA          |
| Congo, Dem. Rep.         | Africa    |  1967|  44.05600|    19941073|     861.5932| NA          |
| Congo, Dem. Rep.         | Africa    |  1972|  45.98900|    23007669|     904.8961| NA          |
| Congo, Dem. Rep.         | Africa    |  1977|  47.80400|    26480870|     795.7573| NA          |
| Congo, Dem. Rep.         | Africa    |  1982|  47.78400|    30646495|     673.7478| NA          |
| Congo, Dem. Rep.         | Africa    |  1987|  47.41200|    35481645|     672.7748| NA          |
| Congo, Dem. Rep.         | Africa    |  1992|  45.54800|    41672143|     457.7192| NA          |
| Congo, Dem. Rep.         | Africa    |  1997|  42.58700|    47798986|     312.1884| NA          |
| Congo, Dem. Rep.         | Africa    |  2002|  44.96600|    55379852|     241.1659| NA          |
| Congo, Dem. Rep.         | Africa    |  2007|  46.46200|    64606759|     277.5519| NA          |
| Congo, Rep.              | Africa    |  1952|  42.11100|      854885|    2125.6214| NA          |
| Congo, Rep.              | Africa    |  1957|  45.05300|      940458|    2315.0566| NA          |
| Congo, Rep.              | Africa    |  1962|  48.43500|     1047924|    2464.7832| NA          |
| Congo, Rep.              | Africa    |  1967|  52.04000|     1179760|    2677.9396| NA          |
| Congo, Rep.              | Africa    |  1972|  54.90700|     1340458|    3213.1527| NA          |
| Congo, Rep.              | Africa    |  1977|  55.62500|     1536769|    3259.1790| NA          |
| Congo, Rep.              | Africa    |  1982|  56.69500|     1774735|    4879.5075| NA          |
| Congo, Rep.              | Africa    |  1987|  57.47000|     2064095|    4201.1949| NA          |
| Congo, Rep.              | Africa    |  1992|  56.43300|     2409073|    4016.2395| NA          |
| Congo, Rep.              | Africa    |  1997|  52.96200|     2800947|    3484.1644| NA          |
| Congo, Rep.              | Africa    |  2002|  52.97000|     3328795|    3484.0620| NA          |
| Congo, Rep.              | Africa    |  2007|  55.32200|     3800610|    3632.5578| NA          |
| Costa Rica               | Americas  |  1952|  57.20600|      926317|    2627.0095| NA          |
| Costa Rica               | Americas  |  1957|  60.02600|     1112300|    2990.0108| NA          |
| Costa Rica               | Americas  |  1962|  62.84200|     1345187|    3460.9370| NA          |
| Costa Rica               | Americas  |  1967|  65.42400|     1588717|    4161.7278| NA          |
| Costa Rica               | Americas  |  1972|  67.84900|     1834796|    5118.1469| NA          |
| Costa Rica               | Americas  |  1977|  70.75000|     2108457|    5926.8770| NA          |
| Costa Rica               | Americas  |  1982|  73.45000|     2424367|    5262.7348| NA          |
| Costa Rica               | Americas  |  1987|  74.75200|     2799811|    5629.9153| NA          |
| Costa Rica               | Americas  |  1992|  75.71300|     3173216|    6160.4163| NA          |
| Costa Rica               | Americas  |  1997|  77.26000|     3518107|    6677.0453| NA          |
| Costa Rica               | Americas  |  2002|  78.12300|     3834934|    7723.4472| NA          |
| Costa Rica               | Americas  |  2007|  78.78200|     4133884|    9645.0614| NA          |
| Cote d'Ivoire            | Africa    |  1952|  40.47700|     2977019|    1388.5947| NA          |
| Cote d'Ivoire            | Africa    |  1957|  42.46900|     3300000|    1500.8959| NA          |
| Cote d'Ivoire            | Africa    |  1962|  44.93000|     3832408|    1728.8694| NA          |
| Cote d'Ivoire            | Africa    |  1967|  47.35000|     4744870|    2052.0505| NA          |
| Cote d'Ivoire            | Africa    |  1972|  49.80100|     6071696|    2378.2011| NA          |
| Cote d'Ivoire            | Africa    |  1977|  52.37400|     7459574|    2517.7365| NA          |
| Cote d'Ivoire            | Africa    |  1982|  53.98300|     9025951|    2602.7102| NA          |
| Cote d'Ivoire            | Africa    |  1987|  54.65500|    10761098|    2156.9561| NA          |
| Cote d'Ivoire            | Africa    |  1992|  52.04400|    12772596|    1648.0738| NA          |
| Cote d'Ivoire            | Africa    |  1997|  47.99100|    14625967|    1786.2654| NA          |
| Cote d'Ivoire            | Africa    |  2002|  46.83200|    16252726|    1648.8008| NA          |
| Cote d'Ivoire            | Africa    |  2007|  48.32800|    18013409|    1544.7501| NA          |
| Croatia                  | Europe    |  1952|  61.21000|     3882229|    3119.2365| NA          |
| Croatia                  | Europe    |  1957|  64.77000|     3991242|    4338.2316| NA          |
| Croatia                  | Europe    |  1962|  67.13000|     4076557|    5477.8900| NA          |
| Croatia                  | Europe    |  1967|  68.50000|     4174366|    6960.2979| NA          |
| Croatia                  | Europe    |  1972|  69.61000|     4225310|    9164.0901| NA          |
| Croatia                  | Europe    |  1977|  70.64000|     4318673|   11305.3852| NA          |
| Croatia                  | Europe    |  1982|  70.46000|     4413368|   13221.8218| NA          |
| Croatia                  | Europe    |  1987|  71.52000|     4484310|   13822.5839| NA          |
| Croatia                  | Europe    |  1992|  72.52700|     4494013|    8447.7949| NA          |
| Croatia                  | Europe    |  1997|  73.68000|     4444595|    9875.6045| NA          |
| Croatia                  | Europe    |  2002|  74.87600|     4481020|   11628.3890| NA          |
| Croatia                  | Europe    |  2007|  75.74800|     4493312|   14619.2227| NA          |
| Cuba                     | Americas  |  1952|  59.42100|     6007797|    5586.5388| NA          |
| Cuba                     | Americas  |  1957|  62.32500|     6640752|    6092.1744| NA          |
| Cuba                     | Americas  |  1962|  65.24600|     7254373|    5180.7559| NA          |
| Cuba                     | Americas  |  1967|  68.29000|     8139332|    5690.2680| NA          |
| Cuba                     | Americas  |  1972|  70.72300|     8831348|    5305.4453| NA          |
| Cuba                     | Americas  |  1977|  72.64900|     9537988|    6380.4950| NA          |
| Cuba                     | Americas  |  1982|  73.71700|     9789224|    7316.9181| NA          |
| Cuba                     | Americas  |  1987|  74.17400|    10239839|    7532.9248| NA          |
| Cuba                     | Americas  |  1992|  74.41400|    10723260|    5592.8440| NA          |
| Cuba                     | Americas  |  1997|  76.15100|    10983007|    5431.9904| NA          |
| Cuba                     | Americas  |  2002|  77.15800|    11226999|    6340.6467| NA          |
| Cuba                     | Americas  |  2007|  78.27300|    11416987|    8948.1029| NA          |
| Czech Republic           | Europe    |  1952|  66.87000|     9125183|    6876.1403| NA          |
| Czech Republic           | Europe    |  1957|  69.03000|     9513758|    8256.3439| NA          |
| Czech Republic           | Europe    |  1962|  69.90000|     9620282|   10136.8671| NA          |
| Czech Republic           | Europe    |  1967|  70.38000|     9835109|   11399.4449| NA          |
| Czech Republic           | Europe    |  1972|  70.29000|     9862158|   13108.4536| NA          |
| Czech Republic           | Europe    |  1977|  70.71000|    10161915|   14800.1606| NA          |
| Czech Republic           | Europe    |  1982|  70.96000|    10303704|   15377.2285| NA          |
| Czech Republic           | Europe    |  1987|  71.58000|    10311597|   16310.4434| NA          |
| Czech Republic           | Europe    |  1992|  72.40000|    10315702|   14297.0212| NA          |
| Czech Republic           | Europe    |  1997|  74.01000|    10300707|   16048.5142| NA          |
| Czech Republic           | Europe    |  2002|  75.51000|    10256295|   17596.2102| NA          |
| Czech Republic           | Europe    |  2007|  76.48600|    10228744|   22833.3085| NA          |
| Denmark                  | Europe    |  1952|  70.78000|     4334000|    9692.3852| NA          |
| Denmark                  | Europe    |  1957|  71.81000|     4487831|   11099.6593| NA          |
| Denmark                  | Europe    |  1962|  72.35000|     4646899|   13583.3135| NA          |
| Denmark                  | Europe    |  1967|  72.96000|     4838800|   15937.2112| NA          |
| Denmark                  | Europe    |  1972|  73.47000|     4991596|   18866.2072| NA          |
| Denmark                  | Europe    |  1977|  74.69000|     5088419|   20422.9015| NA          |
| Denmark                  | Europe    |  1982|  74.63000|     5117810|   21688.0405| NA          |
| Denmark                  | Europe    |  1987|  74.80000|     5127024|   25116.1758| NA          |
| Denmark                  | Europe    |  1992|  75.33000|     5171393|   26406.7399| NA          |
| Denmark                  | Europe    |  1997|  76.11000|     5283663|   29804.3457| NA          |
| Denmark                  | Europe    |  2002|  77.18000|     5374693|   32166.5001| NA          |
| Denmark                  | Europe    |  2007|  78.33200|     5468120|   35278.4187| NA          |
| Djibouti                 | Africa    |  1952|  34.81200|       63149|    2669.5295| NA          |
| Djibouti                 | Africa    |  1957|  37.32800|       71851|    2864.9691| NA          |
| Djibouti                 | Africa    |  1962|  39.69300|       89898|    3020.9893| NA          |
| Djibouti                 | Africa    |  1967|  42.07400|      127617|    3020.0505| NA          |
| Djibouti                 | Africa    |  1972|  44.36600|      178848|    3694.2124| NA          |
| Djibouti                 | Africa    |  1977|  46.51900|      228694|    3081.7610| NA          |
| Djibouti                 | Africa    |  1982|  48.81200|      305991|    2879.4681| NA          |
| Djibouti                 | Africa    |  1987|  50.04000|      311025|    2880.1026| NA          |
| Djibouti                 | Africa    |  1992|  51.60400|      384156|    2377.1562| NA          |
| Djibouti                 | Africa    |  1997|  53.15700|      417908|    1895.0170| NA          |
| Djibouti                 | Africa    |  2002|  53.37300|      447416|    1908.2609| NA          |
| Djibouti                 | Africa    |  2007|  54.79100|      496374|    2082.4816| NA          |
| Dominican Republic       | Americas  |  1952|  45.92800|     2491346|    1397.7171| NA          |
| Dominican Republic       | Americas  |  1957|  49.82800|     2923186|    1544.4030| NA          |
| Dominican Republic       | Americas  |  1962|  53.45900|     3453434|    1662.1374| NA          |
| Dominican Republic       | Americas  |  1967|  56.75100|     4049146|    1653.7230| NA          |
| Dominican Republic       | Americas  |  1972|  59.63100|     4671329|    2189.8745| NA          |
| Dominican Republic       | Americas  |  1977|  61.78800|     5302800|    2681.9889| NA          |
| Dominican Republic       | Americas  |  1982|  63.72700|     5968349|    2861.0924| NA          |
| Dominican Republic       | Americas  |  1987|  66.04600|     6655297|    2899.8422| NA          |
| Dominican Republic       | Americas  |  1992|  68.45700|     7351181|    3044.2142| NA          |
| Dominican Republic       | Americas  |  1997|  69.95700|     7992357|    3614.1013| NA          |
| Dominican Republic       | Americas  |  2002|  70.84700|     8650322|    4563.8082| NA          |
| Dominican Republic       | Americas  |  2007|  72.23500|     9319622|    6025.3748| NA          |
| Ecuador                  | Americas  |  1952|  48.35700|     3548753|    3522.1107| NA          |
| Ecuador                  | Americas  |  1957|  51.35600|     4058385|    3780.5467| NA          |
| Ecuador                  | Americas  |  1962|  54.64000|     4681707|    4086.1141| NA          |
| Ecuador                  | Americas  |  1967|  56.67800|     5432424|    4579.0742| NA          |
| Ecuador                  | Americas  |  1972|  58.79600|     6298651|    5280.9947| NA          |
| Ecuador                  | Americas  |  1977|  61.31000|     7278866|    6679.6233| NA          |
| Ecuador                  | Americas  |  1982|  64.34200|     8365850|    7213.7913| NA          |
| Ecuador                  | Americas  |  1987|  67.23100|     9545158|    6481.7770| NA          |
| Ecuador                  | Americas  |  1992|  69.61300|    10748394|    7103.7026| NA          |
| Ecuador                  | Americas  |  1997|  72.31200|    11911819|    7429.4559| NA          |
| Ecuador                  | Americas  |  2002|  74.17300|    12921234|    5773.0445| NA          |
| Ecuador                  | Americas  |  2007|  74.99400|    13755680|    6873.2623| NA          |
| Egypt                    | Africa    |  1952|  41.89300|    22223309|    1418.8224| NA          |
| Egypt                    | Africa    |  1957|  44.44400|    25009741|    1458.9153| NA          |
| Egypt                    | Africa    |  1962|  46.99200|    28173309|    1693.3359| NA          |
| Egypt                    | Africa    |  1967|  49.29300|    31681188|    1814.8807| NA          |
| Egypt                    | Africa    |  1972|  51.13700|    34807417|    2024.0081| NA          |
| Egypt                    | Africa    |  1977|  53.31900|    38783863|    2785.4936| NA          |
| Egypt                    | Africa    |  1982|  56.00600|    45681811|    3503.7296| NA          |
| Egypt                    | Africa    |  1987|  59.79700|    52799062|    3885.4607| NA          |
| Egypt                    | Africa    |  1992|  63.67400|    59402198|    3794.7552| NA          |
| Egypt                    | Africa    |  1997|  67.21700|    66134291|    4173.1818| NA          |
| Egypt                    | Africa    |  2002|  69.80600|    73312559|    4754.6044| NA          |
| Egypt                    | Africa    |  2007|  71.33800|    80264543|    5581.1810| NA          |
| El Salvador              | Americas  |  1952|  45.26200|     2042865|    3048.3029| NA          |
| El Salvador              | Americas  |  1957|  48.57000|     2355805|    3421.5232| NA          |
| El Salvador              | Americas  |  1962|  52.30700|     2747687|    3776.8036| NA          |
| El Salvador              | Americas  |  1967|  55.85500|     3232927|    4358.5954| NA          |
| El Salvador              | Americas  |  1972|  58.20700|     3790903|    4520.2460| NA          |
| El Salvador              | Americas  |  1977|  56.69600|     4282586|    5138.9224| NA          |
| El Salvador              | Americas  |  1982|  56.60400|     4474873|    4098.3442| NA          |
| El Salvador              | Americas  |  1987|  63.15400|     4842194|    4140.4421| NA          |
| El Salvador              | Americas  |  1992|  66.79800|     5274649|    4444.2317| NA          |
| El Salvador              | Americas  |  1997|  69.53500|     5783439|    5154.8255| NA          |
| El Salvador              | Americas  |  2002|  70.73400|     6353681|    5351.5687| NA          |
| El Salvador              | Americas  |  2007|  71.87800|     6939688|    5728.3535| NA          |
| Equatorial Guinea        | Africa    |  1952|  34.48200|      216964|     375.6431| NA          |
| Equatorial Guinea        | Africa    |  1957|  35.98300|      232922|     426.0964| NA          |
| Equatorial Guinea        | Africa    |  1962|  37.48500|      249220|     582.8420| NA          |
| Equatorial Guinea        | Africa    |  1967|  38.98700|      259864|     915.5960| NA          |
| Equatorial Guinea        | Africa    |  1972|  40.51600|      277603|     672.4123| NA          |
| Equatorial Guinea        | Africa    |  1977|  42.02400|      192675|     958.5668| NA          |
| Equatorial Guinea        | Africa    |  1982|  43.66200|      285483|     927.8253| NA          |
| Equatorial Guinea        | Africa    |  1987|  45.66400|      341244|     966.8968| NA          |
| Equatorial Guinea        | Africa    |  1992|  47.54500|      387838|    1132.0550| NA          |
| Equatorial Guinea        | Africa    |  1997|  48.24500|      439971|    2814.4808| NA          |
| Equatorial Guinea        | Africa    |  2002|  49.34800|      495627|    7703.4959| NA          |
| Equatorial Guinea        | Africa    |  2007|  51.57900|      551201|   12154.0897| NA          |
| Eritrea                  | Africa    |  1952|  35.92800|     1438760|     328.9406| NA          |
| Eritrea                  | Africa    |  1957|  38.04700|     1542611|     344.1619| NA          |
| Eritrea                  | Africa    |  1962|  40.15800|     1666618|     380.9958| NA          |
| Eritrea                  | Africa    |  1967|  42.18900|     1820319|     468.7950| NA          |
| Eritrea                  | Africa    |  1972|  44.14200|     2260187|     514.3242| NA          |
| Eritrea                  | Africa    |  1977|  44.53500|     2512642|     505.7538| NA          |
| Eritrea                  | Africa    |  1982|  43.89000|     2637297|     524.8758| NA          |
| Eritrea                  | Africa    |  1987|  46.45300|     2915959|     521.1341| NA          |
| Eritrea                  | Africa    |  1992|  49.99100|     3668440|     582.8585| NA          |
| Eritrea                  | Africa    |  1997|  53.37800|     4058319|     913.4708| NA          |
| Eritrea                  | Africa    |  2002|  55.24000|     4414865|     765.3500| NA          |
| Eritrea                  | Africa    |  2007|  58.04000|     4906585|     641.3695| NA          |
| Ethiopia                 | Africa    |  1952|  34.07800|    20860941|     362.1463| Addis Ababa |
| Ethiopia                 | Africa    |  1957|  36.66700|    22815614|     378.9042| Addis Ababa |
| Ethiopia                 | Africa    |  1962|  40.05900|    25145372|     419.4564| Addis Ababa |
| Ethiopia                 | Africa    |  1967|  42.11500|    27860297|     516.1186| Addis Ababa |
| Ethiopia                 | Africa    |  1972|  43.51500|    30770372|     566.2439| Addis Ababa |
| Ethiopia                 | Africa    |  1977|  44.51000|    34617799|     556.8084| Addis Ababa |
| Ethiopia                 | Africa    |  1982|  44.91600|    38111756|     577.8607| Addis Ababa |
| Ethiopia                 | Africa    |  1987|  46.68400|    42999530|     573.7413| Addis Ababa |
| Ethiopia                 | Africa    |  1992|  48.09100|    52088559|     421.3535| Addis Ababa |
| Ethiopia                 | Africa    |  1997|  49.40200|    59861301|     515.8894| Addis Ababa |
| Ethiopia                 | Africa    |  2002|  50.72500|    67946797|     530.0535| Addis Ababa |
| Ethiopia                 | Africa    |  2007|  52.94700|    76511887|     690.8056| Addis Ababa |
| Finland                  | Europe    |  1952|  66.55000|     4090500|    6424.5191| NA          |
| Finland                  | Europe    |  1957|  67.49000|     4324000|    7545.4154| NA          |
| Finland                  | Europe    |  1962|  68.75000|     4491443|    9371.8426| NA          |
| Finland                  | Europe    |  1967|  69.83000|     4605744|   10921.6363| NA          |
| Finland                  | Europe    |  1972|  70.87000|     4639657|   14358.8759| NA          |
| Finland                  | Europe    |  1977|  72.52000|     4738902|   15605.4228| NA          |
| Finland                  | Europe    |  1982|  74.55000|     4826933|   18533.1576| NA          |
| Finland                  | Europe    |  1987|  74.83000|     4931729|   21141.0122| NA          |
| Finland                  | Europe    |  1992|  75.70000|     5041039|   20647.1650| NA          |
| Finland                  | Europe    |  1997|  77.13000|     5134406|   23723.9502| NA          |
| Finland                  | Europe    |  2002|  78.37000|     5193039|   28204.5906| NA          |
| Finland                  | Europe    |  2007|  79.31300|     5238460|   33207.0844| NA          |
| France                   | Europe    |  1952|  67.41000|    42459667|    7029.8093| Paris       |
| France                   | Europe    |  1957|  68.93000|    44310863|    8662.8349| Paris       |
| France                   | Europe    |  1962|  70.51000|    47124000|   10560.4855| Paris       |
| France                   | Europe    |  1967|  71.55000|    49569000|   12999.9177| Paris       |
| France                   | Europe    |  1972|  72.38000|    51732000|   16107.1917| Paris       |
| France                   | Europe    |  1977|  73.83000|    53165019|   18292.6351| Paris       |
| France                   | Europe    |  1982|  74.89000|    54433565|   20293.8975| Paris       |
| France                   | Europe    |  1987|  76.34000|    55630100|   22066.4421| Paris       |
| France                   | Europe    |  1992|  77.46000|    57374179|   24703.7961| Paris       |
| France                   | Europe    |  1997|  78.64000|    58623428|   25889.7849| Paris       |
| France                   | Europe    |  2002|  79.59000|    59925035|   28926.0323| Paris       |
| France                   | Europe    |  2007|  80.65700|    61083916|   30470.0167| Paris       |
| Gabon                    | Africa    |  1952|  37.00300|      420702|    4293.4765| NA          |
| Gabon                    | Africa    |  1957|  38.99900|      434904|    4976.1981| NA          |
| Gabon                    | Africa    |  1962|  40.48900|      455661|    6631.4592| NA          |
| Gabon                    | Africa    |  1967|  44.59800|      489004|    8358.7620| NA          |
| Gabon                    | Africa    |  1972|  48.69000|      537977|   11401.9484| NA          |
| Gabon                    | Africa    |  1977|  52.79000|      706367|   21745.5733| NA          |
| Gabon                    | Africa    |  1982|  56.56400|      753874|   15113.3619| NA          |
| Gabon                    | Africa    |  1987|  60.19000|      880397|   11864.4084| NA          |
| Gabon                    | Africa    |  1992|  61.36600|      985739|   13522.1575| NA          |
| Gabon                    | Africa    |  1997|  60.46100|     1126189|   14722.8419| NA          |
| Gabon                    | Africa    |  2002|  56.76100|     1299304|   12521.7139| NA          |
| Gabon                    | Africa    |  2007|  56.73500|     1454867|   13206.4845| NA          |
| Gambia                   | Africa    |  1952|  30.00000|      284320|     485.2307| NA          |
| Gambia                   | Africa    |  1957|  32.06500|      323150|     520.9267| NA          |
| Gambia                   | Africa    |  1962|  33.89600|      374020|     599.6503| NA          |
| Gambia                   | Africa    |  1967|  35.85700|      439593|     734.7829| NA          |
| Gambia                   | Africa    |  1972|  38.30800|      517101|     756.0868| NA          |
| Gambia                   | Africa    |  1977|  41.84200|      608274|     884.7553| NA          |
| Gambia                   | Africa    |  1982|  45.58000|      715523|     835.8096| NA          |
| Gambia                   | Africa    |  1987|  49.26500|      848406|     611.6589| NA          |
| Gambia                   | Africa    |  1992|  52.64400|     1025384|     665.6244| NA          |
| Gambia                   | Africa    |  1997|  55.86100|     1235767|     653.7302| NA          |
| Gambia                   | Africa    |  2002|  58.04100|     1457766|     660.5856| NA          |
| Gambia                   | Africa    |  2007|  59.44800|     1688359|     752.7497| NA          |
| Germany                  | Europe    |  1952|  67.50000|    69145952|    7144.1144| NA          |
| Germany                  | Europe    |  1957|  69.10000|    71019069|   10187.8267| NA          |
| Germany                  | Europe    |  1962|  70.30000|    73739117|   12902.4629| NA          |
| Germany                  | Europe    |  1967|  70.80000|    76368453|   14745.6256| NA          |
| Germany                  | Europe    |  1972|  71.00000|    78717088|   18016.1803| NA          |
| Germany                  | Europe    |  1977|  72.50000|    78160773|   20512.9212| NA          |
| Germany                  | Europe    |  1982|  73.80000|    78335266|   22031.5327| NA          |
| Germany                  | Europe    |  1987|  74.84700|    77718298|   24639.1857| NA          |
| Germany                  | Europe    |  1992|  76.07000|    80597764|   26505.3032| NA          |
| Germany                  | Europe    |  1997|  77.34000|    82011073|   27788.8842| NA          |
| Germany                  | Europe    |  2002|  78.67000|    82350671|   30035.8020| NA          |
| Germany                  | Europe    |  2007|  79.40600|    82400996|   32170.3744| NA          |
| Ghana                    | Africa    |  1952|  43.14900|     5581001|     911.2989| NA          |
| Ghana                    | Africa    |  1957|  44.77900|     6391288|    1043.5615| NA          |
| Ghana                    | Africa    |  1962|  46.45200|     7355248|    1190.0411| NA          |
| Ghana                    | Africa    |  1967|  48.07200|     8490213|    1125.6972| NA          |
| Ghana                    | Africa    |  1972|  49.87500|     9354120|    1178.2237| NA          |
| Ghana                    | Africa    |  1977|  51.75600|    10538093|     993.2240| NA          |
| Ghana                    | Africa    |  1982|  53.74400|    11400338|     876.0326| NA          |
| Ghana                    | Africa    |  1987|  55.72900|    14168101|     847.0061| NA          |
| Ghana                    | Africa    |  1992|  57.50100|    16278738|     925.0602| NA          |
| Ghana                    | Africa    |  1997|  58.55600|    18418288|    1005.2458| NA          |
| Ghana                    | Africa    |  2002|  58.45300|    20550751|    1111.9846| NA          |
| Ghana                    | Africa    |  2007|  60.02200|    22873338|    1327.6089| NA          |
| Greece                   | Europe    |  1952|  65.86000|     7733250|    3530.6901| Athens      |
| Greece                   | Europe    |  1957|  67.86000|     8096218|    4916.2999| Athens      |
| Greece                   | Europe    |  1962|  69.51000|     8448233|    6017.1907| Athens      |
| Greece                   | Europe    |  1967|  71.00000|     8716441|    8513.0970| Athens      |
| Greece                   | Europe    |  1972|  72.34000|     8888628|   12724.8296| Athens      |
| Greece                   | Europe    |  1977|  73.68000|     9308479|   14195.5243| Athens      |
| Greece                   | Europe    |  1982|  75.24000|     9786480|   15268.4209| Athens      |
| Greece                   | Europe    |  1987|  76.67000|     9974490|   16120.5284| Athens      |
| Greece                   | Europe    |  1992|  77.03000|    10325429|   17541.4963| Athens      |
| Greece                   | Europe    |  1997|  77.86900|    10502372|   18747.6981| Athens      |
| Greece                   | Europe    |  2002|  78.25600|    10603863|   22514.2548| Athens      |
| Greece                   | Europe    |  2007|  79.48300|    10706290|   27538.4119| Athens      |
| Guatemala                | Americas  |  1952|  42.02300|     3146381|    2428.2378| NA          |
| Guatemala                | Americas  |  1957|  44.14200|     3640876|    2617.1560| NA          |
| Guatemala                | Americas  |  1962|  46.95400|     4208858|    2750.3644| NA          |
| Guatemala                | Americas  |  1967|  50.01600|     4690773|    3242.5311| NA          |
| Guatemala                | Americas  |  1972|  53.73800|     5149581|    4031.4083| NA          |
| Guatemala                | Americas  |  1977|  56.02900|     5703430|    4879.9927| NA          |
| Guatemala                | Americas  |  1982|  58.13700|     6395630|    4820.4948| NA          |
| Guatemala                | Americas  |  1987|  60.78200|     7326406|    4246.4860| NA          |
| Guatemala                | Americas  |  1992|  63.37300|     8486949|    4439.4508| NA          |
| Guatemala                | Americas  |  1997|  66.32200|     9803875|    4684.3138| NA          |
| Guatemala                | Americas  |  2002|  68.97800|    11178650|    4858.3475| NA          |
| Guatemala                | Americas  |  2007|  70.25900|    12572928|    5186.0500| NA          |
| Guinea                   | Africa    |  1952|  33.60900|     2664249|     510.1965| NA          |
| Guinea                   | Africa    |  1957|  34.55800|     2876726|     576.2670| NA          |
| Guinea                   | Africa    |  1962|  35.75300|     3140003|     686.3737| NA          |
| Guinea                   | Africa    |  1967|  37.19700|     3451418|     708.7595| NA          |
| Guinea                   | Africa    |  1972|  38.84200|     3811387|     741.6662| NA          |
| Guinea                   | Africa    |  1977|  40.76200|     4227026|     874.6859| NA          |
| Guinea                   | Africa    |  1982|  42.89100|     4710497|     857.2504| NA          |
| Guinea                   | Africa    |  1987|  45.55200|     5650262|     805.5725| NA          |
| Guinea                   | Africa    |  1992|  48.57600|     6990574|     794.3484| NA          |
| Guinea                   | Africa    |  1997|  51.45500|     8048834|     869.4498| NA          |
| Guinea                   | Africa    |  2002|  53.67600|     8807818|     945.5836| NA          |
| Guinea                   | Africa    |  2007|  56.00700|     9947814|     942.6542| NA          |
| Guinea-Bissau            | Africa    |  1952|  32.50000|      580653|     299.8503| NA          |
| Guinea-Bissau            | Africa    |  1957|  33.48900|      601095|     431.7905| NA          |
| Guinea-Bissau            | Africa    |  1962|  34.48800|      627820|     522.0344| NA          |
| Guinea-Bissau            | Africa    |  1967|  35.49200|      601287|     715.5806| NA          |
| Guinea-Bissau            | Africa    |  1972|  36.48600|      625361|     820.2246| NA          |
| Guinea-Bissau            | Africa    |  1977|  37.46500|      745228|     764.7260| NA          |
| Guinea-Bissau            | Africa    |  1982|  39.32700|      825987|     838.1240| NA          |
| Guinea-Bissau            | Africa    |  1987|  41.24500|      927524|     736.4154| NA          |
| Guinea-Bissau            | Africa    |  1992|  43.26600|     1050938|     745.5399| NA          |
| Guinea-Bissau            | Africa    |  1997|  44.87300|     1193708|     796.6645| NA          |
| Guinea-Bissau            | Africa    |  2002|  45.50400|     1332459|     575.7047| NA          |
| Guinea-Bissau            | Africa    |  2007|  46.38800|     1472041|     579.2317| NA          |
| Haiti                    | Americas  |  1952|  37.57900|     3201488|    1840.3669| NA          |
| Haiti                    | Americas  |  1957|  40.69600|     3507701|    1726.8879| NA          |
| Haiti                    | Americas  |  1962|  43.59000|     3880130|    1796.5890| NA          |
| Haiti                    | Americas  |  1967|  46.24300|     4318137|    1452.0577| NA          |
| Haiti                    | Americas  |  1972|  48.04200|     4698301|    1654.4569| NA          |
| Haiti                    | Americas  |  1977|  49.92300|     4908554|    1874.2989| NA          |
| Haiti                    | Americas  |  1982|  51.46100|     5198399|    2011.1595| NA          |
| Haiti                    | Americas  |  1987|  53.63600|     5756203|    1823.0160| NA          |
| Haiti                    | Americas  |  1992|  55.08900|     6326682|    1456.3095| NA          |
| Haiti                    | Americas  |  1997|  56.67100|     6913545|    1341.7269| NA          |
| Haiti                    | Americas  |  2002|  58.13700|     7607651|    1270.3649| NA          |
| Haiti                    | Americas  |  2007|  60.91600|     8502814|    1201.6372| NA          |
| Honduras                 | Americas  |  1952|  41.91200|     1517453|    2194.9262| NA          |
| Honduras                 | Americas  |  1957|  44.66500|     1770390|    2220.4877| NA          |
| Honduras                 | Americas  |  1962|  48.04100|     2090162|    2291.1568| NA          |
| Honduras                 | Americas  |  1967|  50.92400|     2500689|    2538.2694| NA          |
| Honduras                 | Americas  |  1972|  53.88400|     2965146|    2529.8423| NA          |
| Honduras                 | Americas  |  1977|  57.40200|     3055235|    3203.2081| NA          |
| Honduras                 | Americas  |  1982|  60.90900|     3669448|    3121.7608| NA          |
| Honduras                 | Americas  |  1987|  64.49200|     4372203|    3023.0967| NA          |
| Honduras                 | Americas  |  1992|  66.39900|     5077347|    3081.6946| NA          |
| Honduras                 | Americas  |  1997|  67.65900|     5867957|    3160.4549| NA          |
| Honduras                 | Americas  |  2002|  68.56500|     6677328|    3099.7287| NA          |
| Honduras                 | Americas  |  2007|  70.19800|     7483763|    3548.3308| NA          |
| Hong Kong, China         | Asia      |  1952|  60.96000|     2125900|    3054.4212| NA          |
| Hong Kong, China         | Asia      |  1957|  64.75000|     2736300|    3629.0765| NA          |
| Hong Kong, China         | Asia      |  1962|  67.65000|     3305200|    4692.6483| NA          |
| Hong Kong, China         | Asia      |  1967|  70.00000|     3722800|    6197.9628| NA          |
| Hong Kong, China         | Asia      |  1972|  72.00000|     4115700|    8315.9281| NA          |
| Hong Kong, China         | Asia      |  1977|  73.60000|     4583700|   11186.1413| NA          |
| Hong Kong, China         | Asia      |  1982|  75.45000|     5264500|   14560.5305| NA          |
| Hong Kong, China         | Asia      |  1987|  76.20000|     5584510|   20038.4727| NA          |
| Hong Kong, China         | Asia      |  1992|  77.60100|     5829696|   24757.6030| NA          |
| Hong Kong, China         | Asia      |  1997|  80.00000|     6495918|   28377.6322| NA          |
| Hong Kong, China         | Asia      |  2002|  81.49500|     6762476|   30209.0152| NA          |
| Hong Kong, China         | Asia      |  2007|  82.20800|     6980412|   39724.9787| NA          |
| Hungary                  | Europe    |  1952|  64.03000|     9504000|    5263.6738| NA          |
| Hungary                  | Europe    |  1957|  66.41000|     9839000|    6040.1800| NA          |
| Hungary                  | Europe    |  1962|  67.96000|    10063000|    7550.3599| NA          |
| Hungary                  | Europe    |  1967|  69.50000|    10223422|    9326.6447| NA          |
| Hungary                  | Europe    |  1972|  69.76000|    10394091|   10168.6561| NA          |
| Hungary                  | Europe    |  1977|  69.95000|    10637171|   11674.8374| NA          |
| Hungary                  | Europe    |  1982|  69.39000|    10705535|   12545.9907| NA          |
| Hungary                  | Europe    |  1987|  69.58000|    10612740|   12986.4800| NA          |
| Hungary                  | Europe    |  1992|  69.17000|    10348684|   10535.6285| NA          |
| Hungary                  | Europe    |  1997|  71.04000|    10244684|   11712.7768| NA          |
| Hungary                  | Europe    |  2002|  72.59000|    10083313|   14843.9356| NA          |
| Hungary                  | Europe    |  2007|  73.33800|     9956108|   18008.9444| NA          |
| Iceland                  | Europe    |  1952|  72.49000|      147962|    7267.6884| NA          |
| Iceland                  | Europe    |  1957|  73.47000|      165110|    9244.0014| NA          |
| Iceland                  | Europe    |  1962|  73.68000|      182053|   10350.1591| NA          |
| Iceland                  | Europe    |  1967|  73.73000|      198676|   13319.8957| NA          |
| Iceland                  | Europe    |  1972|  74.46000|      209275|   15798.0636| NA          |
| Iceland                  | Europe    |  1977|  76.11000|      221823|   19654.9625| NA          |
| Iceland                  | Europe    |  1982|  76.99000|      233997|   23269.6075| NA          |
| Iceland                  | Europe    |  1987|  77.23000|      244676|   26923.2063| NA          |
| Iceland                  | Europe    |  1992|  78.77000|      259012|   25144.3920| NA          |
| Iceland                  | Europe    |  1997|  78.95000|      271192|   28061.0997| NA          |
| Iceland                  | Europe    |  2002|  80.50000|      288030|   31163.2020| NA          |
| Iceland                  | Europe    |  2007|  81.75700|      301931|   36180.7892| NA          |
| India                    | Asia      |  1952|  37.37300|   372000000|     546.5657| NA          |
| India                    | Asia      |  1957|  40.24900|   409000000|     590.0620| NA          |
| India                    | Asia      |  1962|  43.60500|   454000000|     658.3472| NA          |
| India                    | Asia      |  1967|  47.19300|   506000000|     700.7706| NA          |
| India                    | Asia      |  1972|  50.65100|   567000000|     724.0325| NA          |
| India                    | Asia      |  1977|  54.20800|   634000000|     813.3373| NA          |
| India                    | Asia      |  1982|  56.59600|   708000000|     855.7235| NA          |
| India                    | Asia      |  1987|  58.55300|   788000000|     976.5127| NA          |
| India                    | Asia      |  1992|  60.22300|   872000000|    1164.4068| NA          |
| India                    | Asia      |  1997|  61.76500|   959000000|    1458.8174| NA          |
| India                    | Asia      |  2002|  62.87900|  1034172547|    1746.7695| NA          |
| India                    | Asia      |  2007|  64.69800|  1110396331|    2452.2104| NA          |
| Indonesia                | Asia      |  1952|  37.46800|    82052000|     749.6817| NA          |
| Indonesia                | Asia      |  1957|  39.91800|    90124000|     858.9003| NA          |
| Indonesia                | Asia      |  1962|  42.51800|    99028000|     849.2898| NA          |
| Indonesia                | Asia      |  1967|  45.96400|   109343000|     762.4318| NA          |
| Indonesia                | Asia      |  1972|  49.20300|   121282000|    1111.1079| NA          |
| Indonesia                | Asia      |  1977|  52.70200|   136725000|    1382.7021| NA          |
| Indonesia                | Asia      |  1982|  56.15900|   153343000|    1516.8730| NA          |
| Indonesia                | Asia      |  1987|  60.13700|   169276000|    1748.3570| NA          |
| Indonesia                | Asia      |  1992|  62.68100|   184816000|    2383.1409| NA          |
| Indonesia                | Asia      |  1997|  66.04100|   199278000|    3119.3356| NA          |
| Indonesia                | Asia      |  2002|  68.58800|   211060000|    2873.9129| NA          |
| Indonesia                | Asia      |  2007|  70.65000|   223547000|    3540.6516| NA          |
| Iran                     | Asia      |  1952|  44.86900|    17272000|    3035.3260| NA          |
| Iran                     | Asia      |  1957|  47.18100|    19792000|    3290.2576| NA          |
| Iran                     | Asia      |  1962|  49.32500|    22874000|    4187.3298| NA          |
| Iran                     | Asia      |  1967|  52.46900|    26538000|    5906.7318| NA          |
| Iran                     | Asia      |  1972|  55.23400|    30614000|    9613.8186| NA          |
| Iran                     | Asia      |  1977|  57.70200|    35480679|   11888.5951| NA          |
| Iran                     | Asia      |  1982|  59.62000|    43072751|    7608.3346| NA          |
| Iran                     | Asia      |  1987|  63.04000|    51889696|    6642.8814| NA          |
| Iran                     | Asia      |  1992|  65.74200|    60397973|    7235.6532| NA          |
| Iran                     | Asia      |  1997|  68.04200|    63327987|    8263.5903| NA          |
| Iran                     | Asia      |  2002|  69.45100|    66907826|    9240.7620| NA          |
| Iran                     | Asia      |  2007|  70.96400|    69453570|   11605.7145| NA          |
| Iraq                     | Asia      |  1952|  45.32000|     5441766|    4129.7661| NA          |
| Iraq                     | Asia      |  1957|  48.43700|     6248643|    6229.3336| NA          |
| Iraq                     | Asia      |  1962|  51.45700|     7240260|    8341.7378| NA          |
| Iraq                     | Asia      |  1967|  54.45900|     8519282|    8931.4598| NA          |
| Iraq                     | Asia      |  1972|  56.95000|    10061506|    9576.0376| NA          |
| Iraq                     | Asia      |  1977|  60.41300|    11882916|   14688.2351| NA          |
| Iraq                     | Asia      |  1982|  62.03800|    14173318|   14517.9071| NA          |
| Iraq                     | Asia      |  1987|  65.04400|    16543189|   11643.5727| NA          |
| Iraq                     | Asia      |  1992|  59.46100|    17861905|    3745.6407| NA          |
| Iraq                     | Asia      |  1997|  58.81100|    20775703|    3076.2398| NA          |
| Iraq                     | Asia      |  2002|  57.04600|    24001816|    4390.7173| NA          |
| Iraq                     | Asia      |  2007|  59.54500|    27499638|    4471.0619| NA          |
| Ireland                  | Europe    |  1952|  66.91000|     2952156|    5210.2803| NA          |
| Ireland                  | Europe    |  1957|  68.90000|     2878220|    5599.0779| NA          |
| Ireland                  | Europe    |  1962|  70.29000|     2830000|    6631.5973| NA          |
| Ireland                  | Europe    |  1967|  71.08000|     2900100|    7655.5690| NA          |
| Ireland                  | Europe    |  1972|  71.28000|     3024400|    9530.7729| NA          |
| Ireland                  | Europe    |  1977|  72.03000|     3271900|   11150.9811| NA          |
| Ireland                  | Europe    |  1982|  73.10000|     3480000|   12618.3214| NA          |
| Ireland                  | Europe    |  1987|  74.36000|     3539900|   13872.8665| NA          |
| Ireland                  | Europe    |  1992|  75.46700|     3557761|   17558.8155| NA          |
| Ireland                  | Europe    |  1997|  76.12200|     3667233|   24521.9471| NA          |
| Ireland                  | Europe    |  2002|  77.78300|     3879155|   34077.0494| NA          |
| Ireland                  | Europe    |  2007|  78.88500|     4109086|   40675.9964| NA          |
| Israel                   | Asia      |  1952|  65.39000|     1620914|    4086.5221| NA          |
| Israel                   | Asia      |  1957|  67.84000|     1944401|    5385.2785| NA          |
| Israel                   | Asia      |  1962|  69.39000|     2310904|    7105.6307| NA          |
| Israel                   | Asia      |  1967|  70.75000|     2693585|    8393.7414| NA          |
| Israel                   | Asia      |  1972|  71.63000|     3095893|   12786.9322| NA          |
| Israel                   | Asia      |  1977|  73.06000|     3495918|   13306.6192| NA          |
| Israel                   | Asia      |  1982|  74.45000|     3858421|   15367.0292| NA          |
| Israel                   | Asia      |  1987|  75.60000|     4203148|   17122.4799| NA          |
| Israel                   | Asia      |  1992|  76.93000|     4936550|   18051.5225| NA          |
| Israel                   | Asia      |  1997|  78.26900|     5531387|   20896.6092| NA          |
| Israel                   | Asia      |  2002|  79.69600|     6029529|   21905.5951| NA          |
| Israel                   | Asia      |  2007|  80.74500|     6426679|   25523.2771| NA          |
| Italy                    | Europe    |  1952|  65.94000|    47666000|    4931.4042| NA          |
| Italy                    | Europe    |  1957|  67.81000|    49182000|    6248.6562| NA          |
| Italy                    | Europe    |  1962|  69.24000|    50843200|    8243.5823| NA          |
| Italy                    | Europe    |  1967|  71.06000|    52667100|   10022.4013| NA          |
| Italy                    | Europe    |  1972|  72.19000|    54365564|   12269.2738| NA          |
| Italy                    | Europe    |  1977|  73.48000|    56059245|   14255.9847| NA          |
| Italy                    | Europe    |  1982|  74.98000|    56535636|   16537.4835| NA          |
| Italy                    | Europe    |  1987|  76.42000|    56729703|   19207.2348| NA          |
| Italy                    | Europe    |  1992|  77.44000|    56840847|   22013.6449| NA          |
| Italy                    | Europe    |  1997|  78.82000|    57479469|   24675.0245| NA          |
| Italy                    | Europe    |  2002|  80.24000|    57926999|   27968.0982| NA          |
| Italy                    | Europe    |  2007|  80.54600|    58147733|   28569.7197| NA          |
| Jamaica                  | Americas  |  1952|  58.53000|     1426095|    2898.5309| NA          |
| Jamaica                  | Americas  |  1957|  62.61000|     1535090|    4756.5258| NA          |
| Jamaica                  | Americas  |  1962|  65.61000|     1665128|    5246.1075| NA          |
| Jamaica                  | Americas  |  1967|  67.51000|     1861096|    6124.7035| NA          |
| Jamaica                  | Americas  |  1972|  69.00000|     1997616|    7433.8893| NA          |
| Jamaica                  | Americas  |  1977|  70.11000|     2156814|    6650.1956| NA          |
| Jamaica                  | Americas  |  1982|  71.21000|     2298309|    6068.0513| NA          |
| Jamaica                  | Americas  |  1987|  71.77000|     2326606|    6351.2375| NA          |
| Jamaica                  | Americas  |  1992|  71.76600|     2378618|    7404.9237| NA          |
| Jamaica                  | Americas  |  1997|  72.26200|     2531311|    7121.9247| NA          |
| Jamaica                  | Americas  |  2002|  72.04700|     2664659|    6994.7749| NA          |
| Jamaica                  | Americas  |  2007|  72.56700|     2780132|    7320.8803| NA          |
| Japan                    | Asia      |  1952|  63.03000|    86459025|    3216.9563| Tokyo       |
| Japan                    | Asia      |  1957|  65.50000|    91563009|    4317.6944| Tokyo       |
| Japan                    | Asia      |  1962|  68.73000|    95831757|    6576.6495| Tokyo       |
| Japan                    | Asia      |  1967|  71.43000|   100825279|    9847.7886| Tokyo       |
| Japan                    | Asia      |  1972|  73.42000|   107188273|   14778.7864| Tokyo       |
| Japan                    | Asia      |  1977|  75.38000|   113872473|   16610.3770| Tokyo       |
| Japan                    | Asia      |  1982|  77.11000|   118454974|   19384.1057| Tokyo       |
| Japan                    | Asia      |  1987|  78.67000|   122091325|   22375.9419| Tokyo       |
| Japan                    | Asia      |  1992|  79.36000|   124329269|   26824.8951| Tokyo       |
| Japan                    | Asia      |  1997|  80.69000|   125956499|   28816.5850| Tokyo       |
| Japan                    | Asia      |  2002|  82.00000|   127065841|   28604.5919| Tokyo       |
| Japan                    | Asia      |  2007|  82.60300|   127467972|   31656.0681| Tokyo       |
| Jordan                   | Asia      |  1952|  43.15800|      607914|    1546.9078| NA          |
| Jordan                   | Asia      |  1957|  45.66900|      746559|    1886.0806| NA          |
| Jordan                   | Asia      |  1962|  48.12600|      933559|    2348.0092| NA          |
| Jordan                   | Asia      |  1967|  51.62900|     1255058|    2741.7963| NA          |
| Jordan                   | Asia      |  1972|  56.52800|     1613551|    2110.8563| NA          |
| Jordan                   | Asia      |  1977|  61.13400|     1937652|    2852.3516| NA          |
| Jordan                   | Asia      |  1982|  63.73900|     2347031|    4161.4160| NA          |
| Jordan                   | Asia      |  1987|  65.86900|     2820042|    4448.6799| NA          |
| Jordan                   | Asia      |  1992|  68.01500|     3867409|    3431.5936| NA          |
| Jordan                   | Asia      |  1997|  69.77200|     4526235|    3645.3796| NA          |
| Jordan                   | Asia      |  2002|  71.26300|     5307470|    3844.9172| NA          |
| Jordan                   | Asia      |  2007|  72.53500|     6053193|    4519.4612| NA          |
| Kenya                    | Africa    |  1952|  42.27000|     6464046|     853.5409| NA          |
| Kenya                    | Africa    |  1957|  44.68600|     7454779|     944.4383| NA          |
| Kenya                    | Africa    |  1962|  47.94900|     8678557|     896.9664| NA          |
| Kenya                    | Africa    |  1967|  50.65400|    10191512|    1056.7365| NA          |
| Kenya                    | Africa    |  1972|  53.55900|    12044785|    1222.3600| NA          |
| Kenya                    | Africa    |  1977|  56.15500|    14500404|    1267.6132| NA          |
| Kenya                    | Africa    |  1982|  58.76600|    17661452|    1348.2258| NA          |
| Kenya                    | Africa    |  1987|  59.33900|    21198082|    1361.9369| NA          |
| Kenya                    | Africa    |  1992|  59.28500|    25020539|    1341.9217| NA          |
| Kenya                    | Africa    |  1997|  54.40700|    28263827|    1360.4850| NA          |
| Kenya                    | Africa    |  2002|  50.99200|    31386842|    1287.5147| NA          |
| Kenya                    | Africa    |  2007|  54.11000|    35610177|    1463.2493| NA          |
| Korea, Dem. Rep.         | Asia      |  1952|  50.05600|     8865488|    1088.2778| NA          |
| Korea, Dem. Rep.         | Asia      |  1957|  54.08100|     9411381|    1571.1347| NA          |
| Korea, Dem. Rep.         | Asia      |  1962|  56.65600|    10917494|    1621.6936| NA          |
| Korea, Dem. Rep.         | Asia      |  1967|  59.94200|    12617009|    2143.5406| NA          |
| Korea, Dem. Rep.         | Asia      |  1972|  63.98300|    14781241|    3701.6215| NA          |
| Korea, Dem. Rep.         | Asia      |  1977|  67.15900|    16325320|    4106.3012| NA          |
| Korea, Dem. Rep.         | Asia      |  1982|  69.10000|    17647518|    4106.5253| NA          |
| Korea, Dem. Rep.         | Asia      |  1987|  70.64700|    19067554|    4106.4923| NA          |
| Korea, Dem. Rep.         | Asia      |  1992|  69.97800|    20711375|    3726.0635| NA          |
| Korea, Dem. Rep.         | Asia      |  1997|  67.72700|    21585105|    1690.7568| NA          |
| Korea, Dem. Rep.         | Asia      |  2002|  66.66200|    22215365|    1646.7582| NA          |
| Korea, Dem. Rep.         | Asia      |  2007|  67.29700|    23301725|    1593.0655| NA          |
| Korea, Rep.              | Asia      |  1952|  47.45300|    20947571|    1030.5922| NA          |
| Korea, Rep.              | Asia      |  1957|  52.68100|    22611552|    1487.5935| NA          |
| Korea, Rep.              | Asia      |  1962|  55.29200|    26420307|    1536.3444| NA          |
| Korea, Rep.              | Asia      |  1967|  57.71600|    30131000|    2029.2281| NA          |
| Korea, Rep.              | Asia      |  1972|  62.61200|    33505000|    3030.8767| NA          |
| Korea, Rep.              | Asia      |  1977|  64.76600|    36436000|    4657.2210| NA          |
| Korea, Rep.              | Asia      |  1982|  67.12300|    39326000|    5622.9425| NA          |
| Korea, Rep.              | Asia      |  1987|  69.81000|    41622000|    8533.0888| NA          |
| Korea, Rep.              | Asia      |  1992|  72.24400|    43805450|   12104.2787| NA          |
| Korea, Rep.              | Asia      |  1997|  74.64700|    46173816|   15993.5280| NA          |
| Korea, Rep.              | Asia      |  2002|  77.04500|    47969150|   19233.9882| NA          |
| Korea, Rep.              | Asia      |  2007|  78.62300|    49044790|   23348.1397| NA          |
| Kuwait                   | Asia      |  1952|  55.56500|      160000|  108382.3529| NA          |
| Kuwait                   | Asia      |  1957|  58.03300|      212846|  113523.1329| NA          |
| Kuwait                   | Asia      |  1962|  60.47000|      358266|   95458.1118| NA          |
| Kuwait                   | Asia      |  1967|  64.62400|      575003|   80894.8833| NA          |
| Kuwait                   | Asia      |  1972|  67.71200|      841934|  109347.8670| NA          |
| Kuwait                   | Asia      |  1977|  69.34300|     1140357|   59265.4771| NA          |
| Kuwait                   | Asia      |  1982|  71.30900|     1497494|   31354.0357| NA          |
| Kuwait                   | Asia      |  1987|  74.17400|     1891487|   28118.4300| NA          |
| Kuwait                   | Asia      |  1992|  75.19000|     1418095|   34932.9196| NA          |
| Kuwait                   | Asia      |  1997|  76.15600|     1765345|   40300.6200| NA          |
| Kuwait                   | Asia      |  2002|  76.90400|     2111561|   35110.1057| NA          |
| Kuwait                   | Asia      |  2007|  77.58800|     2505559|   47306.9898| NA          |
| Lebanon                  | Asia      |  1952|  55.92800|     1439529|    4834.8041| NA          |
| Lebanon                  | Asia      |  1957|  59.48900|     1647412|    6089.7869| NA          |
| Lebanon                  | Asia      |  1962|  62.09400|     1886848|    5714.5606| NA          |
| Lebanon                  | Asia      |  1967|  63.87000|     2186894|    6006.9830| NA          |
| Lebanon                  | Asia      |  1972|  65.42100|     2680018|    7486.3843| NA          |
| Lebanon                  | Asia      |  1977|  66.09900|     3115787|    8659.6968| NA          |
| Lebanon                  | Asia      |  1982|  66.98300|     3086876|    7640.5195| NA          |
| Lebanon                  | Asia      |  1987|  67.92600|     3089353|    5377.0913| NA          |
| Lebanon                  | Asia      |  1992|  69.29200|     3219994|    6890.8069| NA          |
| Lebanon                  | Asia      |  1997|  70.26500|     3430388|    8754.9639| NA          |
| Lebanon                  | Asia      |  2002|  71.02800|     3677780|    9313.9388| NA          |
| Lebanon                  | Asia      |  2007|  71.99300|     3921278|   10461.0587| NA          |
| Lesotho                  | Africa    |  1952|  42.13800|      748747|     298.8462| NA          |
| Lesotho                  | Africa    |  1957|  45.04700|      813338|     335.9971| NA          |
| Lesotho                  | Africa    |  1962|  47.74700|      893143|     411.8006| NA          |
| Lesotho                  | Africa    |  1967|  48.49200|      996380|     498.6390| NA          |
| Lesotho                  | Africa    |  1972|  49.76700|     1116779|     496.5816| NA          |
| Lesotho                  | Africa    |  1977|  52.20800|     1251524|     745.3695| NA          |
| Lesotho                  | Africa    |  1982|  55.07800|     1411807|     797.2631| NA          |
| Lesotho                  | Africa    |  1987|  57.18000|     1599200|     773.9932| NA          |
| Lesotho                  | Africa    |  1992|  59.68500|     1803195|     977.4863| NA          |
| Lesotho                  | Africa    |  1997|  55.55800|     1982823|    1186.1480| NA          |
| Lesotho                  | Africa    |  2002|  44.59300|     2046772|    1275.1846| NA          |
| Lesotho                  | Africa    |  2007|  42.59200|     2012649|    1569.3314| NA          |
| Liberia                  | Africa    |  1952|  38.48000|      863308|     575.5730| NA          |
| Liberia                  | Africa    |  1957|  39.48600|      975950|     620.9700| NA          |
| Liberia                  | Africa    |  1962|  40.50200|     1112796|     634.1952| NA          |
| Liberia                  | Africa    |  1967|  41.53600|     1279406|     713.6036| NA          |
| Liberia                  | Africa    |  1972|  42.61400|     1482628|     803.0055| NA          |
| Liberia                  | Africa    |  1977|  43.76400|     1703617|     640.3224| NA          |
| Liberia                  | Africa    |  1982|  44.85200|     1956875|     572.1996| NA          |
| Liberia                  | Africa    |  1987|  46.02700|     2269414|     506.1139| NA          |
| Liberia                  | Africa    |  1992|  40.80200|     1912974|     636.6229| NA          |
| Liberia                  | Africa    |  1997|  42.22100|     2200725|     609.1740| NA          |
| Liberia                  | Africa    |  2002|  43.75300|     2814651|     531.4824| NA          |
| Liberia                  | Africa    |  2007|  45.67800|     3193942|     414.5073| NA          |
| Libya                    | Africa    |  1952|  42.72300|     1019729|    2387.5481| NA          |
| Libya                    | Africa    |  1957|  45.28900|     1201578|    3448.2844| NA          |
| Libya                    | Africa    |  1962|  47.80800|     1441863|    6757.0308| NA          |
| Libya                    | Africa    |  1967|  50.22700|     1759224|   18772.7517| NA          |
| Libya                    | Africa    |  1972|  52.77300|     2183877|   21011.4972| NA          |
| Libya                    | Africa    |  1977|  57.44200|     2721783|   21951.2118| NA          |
| Libya                    | Africa    |  1982|  62.15500|     3344074|   17364.2754| NA          |
| Libya                    | Africa    |  1987|  66.23400|     3799845|   11770.5898| NA          |
| Libya                    | Africa    |  1992|  68.75500|     4364501|    9640.1385| NA          |
| Libya                    | Africa    |  1997|  71.55500|     4759670|    9467.4461| NA          |
| Libya                    | Africa    |  2002|  72.73700|     5368585|    9534.6775| NA          |
| Libya                    | Africa    |  2007|  73.95200|     6036914|   12057.4993| NA          |
| Madagascar               | Africa    |  1952|  36.68100|     4762912|    1443.0117| NA          |
| Madagascar               | Africa    |  1957|  38.86500|     5181679|    1589.2027| NA          |
| Madagascar               | Africa    |  1962|  40.84800|     5703324|    1643.3871| NA          |
| Madagascar               | Africa    |  1967|  42.88100|     6334556|    1634.0473| NA          |
| Madagascar               | Africa    |  1972|  44.85100|     7082430|    1748.5630| NA          |
| Madagascar               | Africa    |  1977|  46.88100|     8007166|    1544.2286| NA          |
| Madagascar               | Africa    |  1982|  48.96900|     9171477|    1302.8787| NA          |
| Madagascar               | Africa    |  1987|  49.35000|    10568642|    1155.4419| NA          |
| Madagascar               | Africa    |  1992|  52.21400|    12210395|    1040.6762| NA          |
| Madagascar               | Africa    |  1997|  54.97800|    14165114|     986.2959| NA          |
| Madagascar               | Africa    |  2002|  57.28600|    16473477|     894.6371| NA          |
| Madagascar               | Africa    |  2007|  59.44300|    19167654|    1044.7701| NA          |
| Malawi                   | Africa    |  1952|  36.25600|     2917802|     369.1651| NA          |
| Malawi                   | Africa    |  1957|  37.20700|     3221238|     416.3698| NA          |
| Malawi                   | Africa    |  1962|  38.41000|     3628608|     427.9011| NA          |
| Malawi                   | Africa    |  1967|  39.48700|     4147252|     495.5148| NA          |
| Malawi                   | Africa    |  1972|  41.76600|     4730997|     584.6220| NA          |
| Malawi                   | Africa    |  1977|  43.76700|     5637246|     663.2237| NA          |
| Malawi                   | Africa    |  1982|  45.64200|     6502825|     632.8039| NA          |
| Malawi                   | Africa    |  1987|  47.45700|     7824747|     635.5174| NA          |
| Malawi                   | Africa    |  1992|  49.42000|    10014249|     563.2000| NA          |
| Malawi                   | Africa    |  1997|  47.49500|    10419991|     692.2758| NA          |
| Malawi                   | Africa    |  2002|  45.00900|    11824495|     665.4231| NA          |
| Malawi                   | Africa    |  2007|  48.30300|    13327079|     759.3499| NA          |
| Malaysia                 | Asia      |  1952|  48.46300|     6748378|    1831.1329| NA          |
| Malaysia                 | Asia      |  1957|  52.10200|     7739235|    1810.0670| NA          |
| Malaysia                 | Asia      |  1962|  55.73700|     8906385|    2036.8849| NA          |
| Malaysia                 | Asia      |  1967|  59.37100|    10154878|    2277.7424| NA          |
| Malaysia                 | Asia      |  1972|  63.01000|    11441462|    2849.0948| NA          |
| Malaysia                 | Asia      |  1977|  65.25600|    12845381|    3827.9216| NA          |
| Malaysia                 | Asia      |  1982|  68.00000|    14441916|    4920.3560| NA          |
| Malaysia                 | Asia      |  1987|  69.50000|    16331785|    5249.8027| NA          |
| Malaysia                 | Asia      |  1992|  70.69300|    18319502|    7277.9128| NA          |
| Malaysia                 | Asia      |  1997|  71.93800|    20476091|   10132.9096| NA          |
| Malaysia                 | Asia      |  2002|  73.04400|    22662365|   10206.9779| NA          |
| Malaysia                 | Asia      |  2007|  74.24100|    24821286|   12451.6558| NA          |
| Mali                     | Africa    |  1952|  33.68500|     3838168|     452.3370| NA          |
| Mali                     | Africa    |  1957|  35.30700|     4241884|     490.3822| NA          |
| Mali                     | Africa    |  1962|  36.93600|     4690372|     496.1743| NA          |
| Mali                     | Africa    |  1967|  38.48700|     5212416|     545.0099| NA          |
| Mali                     | Africa    |  1972|  39.97700|     5828158|     581.3689| NA          |
| Mali                     | Africa    |  1977|  41.71400|     6491649|     686.3953| NA          |
| Mali                     | Africa    |  1982|  43.91600|     6998256|     618.0141| NA          |
| Mali                     | Africa    |  1987|  46.36400|     7634008|     684.1716| NA          |
| Mali                     | Africa    |  1992|  48.38800|     8416215|     739.0144| NA          |
| Mali                     | Africa    |  1997|  49.90300|     9384984|     790.2580| NA          |
| Mali                     | Africa    |  2002|  51.81800|    10580176|     951.4098| NA          |
| Mali                     | Africa    |  2007|  54.46700|    12031795|    1042.5816| NA          |
| Mauritania               | Africa    |  1952|  40.54300|     1022556|     743.1159| NA          |
| Mauritania               | Africa    |  1957|  42.33800|     1076852|     846.1203| NA          |
| Mauritania               | Africa    |  1962|  44.24800|     1146757|    1055.8960| NA          |
| Mauritania               | Africa    |  1967|  46.28900|     1230542|    1421.1452| NA          |
| Mauritania               | Africa    |  1972|  48.43700|     1332786|    1586.8518| NA          |
| Mauritania               | Africa    |  1977|  50.85200|     1456688|    1497.4922| NA          |
| Mauritania               | Africa    |  1982|  53.59900|     1622136|    1481.1502| NA          |
| Mauritania               | Africa    |  1987|  56.14500|     1841240|    1421.6036| NA          |
| Mauritania               | Africa    |  1992|  58.33300|     2119465|    1361.3698| NA          |
| Mauritania               | Africa    |  1997|  60.43000|     2444741|    1483.1361| NA          |
| Mauritania               | Africa    |  2002|  62.24700|     2828858|    1579.0195| NA          |
| Mauritania               | Africa    |  2007|  64.16400|     3270065|    1803.1515| NA          |
| Mauritius                | Africa    |  1952|  50.98600|      516556|    1967.9557| NA          |
| Mauritius                | Africa    |  1957|  58.08900|      609816|    2034.0380| NA          |
| Mauritius                | Africa    |  1962|  60.24600|      701016|    2529.0675| NA          |
| Mauritius                | Africa    |  1967|  61.55700|      789309|    2475.3876| NA          |
| Mauritius                | Africa    |  1972|  62.94400|      851334|    2575.4842| NA          |
| Mauritius                | Africa    |  1977|  64.93000|      913025|    3710.9830| NA          |
| Mauritius                | Africa    |  1982|  66.71100|      992040|    3688.0377| NA          |
| Mauritius                | Africa    |  1987|  68.74000|     1042663|    4783.5869| NA          |
| Mauritius                | Africa    |  1992|  69.74500|     1096202|    6058.2538| NA          |
| Mauritius                | Africa    |  1997|  70.73600|     1149818|    7425.7053| NA          |
| Mauritius                | Africa    |  2002|  71.95400|     1200206|    9021.8159| NA          |
| Mauritius                | Africa    |  2007|  72.80100|     1250882|   10956.9911| NA          |
| Mexico                   | Americas  |  1952|  50.78900|    30144317|    3478.1255| Mexico City |
| Mexico                   | Americas  |  1957|  55.19000|    35015548|    4131.5466| Mexico City |
| Mexico                   | Americas  |  1962|  58.29900|    41121485|    4581.6094| Mexico City |
| Mexico                   | Americas  |  1967|  60.11000|    47995559|    5754.7339| Mexico City |
| Mexico                   | Americas  |  1972|  62.36100|    55984294|    6809.4067| Mexico City |
| Mexico                   | Americas  |  1977|  65.03200|    63759976|    7674.9291| Mexico City |
| Mexico                   | Americas  |  1982|  67.40500|    71640904|    9611.1475| Mexico City |
| Mexico                   | Americas  |  1987|  69.49800|    80122492|    8688.1560| Mexico City |
| Mexico                   | Americas  |  1992|  71.45500|    88111030|    9472.3843| Mexico City |
| Mexico                   | Americas  |  1997|  73.67000|    95895146|    9767.2975| Mexico City |
| Mexico                   | Americas  |  2002|  74.90200|   102479927|   10742.4405| Mexico City |
| Mexico                   | Americas  |  2007|  76.19500|   108700891|   11977.5750| Mexico City |
| Mongolia                 | Asia      |  1952|  42.24400|      800663|     786.5669| NA          |
| Mongolia                 | Asia      |  1957|  45.24800|      882134|     912.6626| NA          |
| Mongolia                 | Asia      |  1962|  48.25100|     1010280|    1056.3540| NA          |
| Mongolia                 | Asia      |  1967|  51.25300|     1149500|    1226.0411| NA          |
| Mongolia                 | Asia      |  1972|  53.75400|     1320500|    1421.7420| NA          |
| Mongolia                 | Asia      |  1977|  55.49100|     1528000|    1647.5117| NA          |
| Mongolia                 | Asia      |  1982|  57.48900|     1756032|    2000.6031| NA          |
| Mongolia                 | Asia      |  1987|  60.22200|     2015133|    2338.0083| NA          |
| Mongolia                 | Asia      |  1992|  61.27100|     2312802|    1785.4020| NA          |
| Mongolia                 | Asia      |  1997|  63.62500|     2494803|    1902.2521| NA          |
| Mongolia                 | Asia      |  2002|  65.03300|     2674234|    2140.7393| NA          |
| Mongolia                 | Asia      |  2007|  66.80300|     2874127|    3095.7723| NA          |
| Montenegro               | Europe    |  1952|  59.16400|      413834|    2647.5856| NA          |
| Montenegro               | Europe    |  1957|  61.44800|      442829|    3682.2599| NA          |
| Montenegro               | Europe    |  1962|  63.72800|      474528|    4649.5938| NA          |
| Montenegro               | Europe    |  1967|  67.17800|      501035|    5907.8509| NA          |
| Montenegro               | Europe    |  1972|  70.63600|      527678|    7778.4140| NA          |
| Montenegro               | Europe    |  1977|  73.06600|      560073|    9595.9299| NA          |
| Montenegro               | Europe    |  1982|  74.10100|      562548|   11222.5876| NA          |
| Montenegro               | Europe    |  1987|  74.86500|      569473|   11732.5102| NA          |
| Montenegro               | Europe    |  1992|  75.43500|      621621|    7003.3390| NA          |
| Montenegro               | Europe    |  1997|  75.44500|      692651|    6465.6133| NA          |
| Montenegro               | Europe    |  2002|  73.98100|      720230|    6557.1943| NA          |
| Montenegro               | Europe    |  2007|  74.54300|      684736|    9253.8961| NA          |
| Morocco                  | Africa    |  1952|  42.87300|     9939217|    1688.2036| NA          |
| Morocco                  | Africa    |  1957|  45.42300|    11406350|    1642.0023| NA          |
| Morocco                  | Africa    |  1962|  47.92400|    13056604|    1566.3535| NA          |
| Morocco                  | Africa    |  1967|  50.33500|    14770296|    1711.0448| NA          |
| Morocco                  | Africa    |  1972|  52.86200|    16660670|    1930.1950| NA          |
| Morocco                  | Africa    |  1977|  55.73000|    18396941|    2370.6200| NA          |
| Morocco                  | Africa    |  1982|  59.65000|    20198730|    2702.6204| NA          |
| Morocco                  | Africa    |  1987|  62.67700|    22987397|    2755.0470| NA          |
| Morocco                  | Africa    |  1992|  65.39300|    25798239|    2948.0473| NA          |
| Morocco                  | Africa    |  1997|  67.66000|    28529501|    2982.1019| NA          |
| Morocco                  | Africa    |  2002|  69.61500|    31167783|    3258.4956| NA          |
| Morocco                  | Africa    |  2007|  71.16400|    33757175|    3820.1752| NA          |
| Mozambique               | Africa    |  1952|  31.28600|     6446316|     468.5260| NA          |
| Mozambique               | Africa    |  1957|  33.77900|     7038035|     495.5868| NA          |
| Mozambique               | Africa    |  1962|  36.16100|     7788944|     556.6864| NA          |
| Mozambique               | Africa    |  1967|  38.11300|     8680909|     566.6692| NA          |
| Mozambique               | Africa    |  1972|  40.32800|     9809596|     724.9178| NA          |
| Mozambique               | Africa    |  1977|  42.49500|    11127868|     502.3197| NA          |
| Mozambique               | Africa    |  1982|  42.79500|    12587223|     462.2114| NA          |
| Mozambique               | Africa    |  1987|  42.86100|    12891952|     389.8762| NA          |
| Mozambique               | Africa    |  1992|  44.28400|    13160731|     410.8968| NA          |
| Mozambique               | Africa    |  1997|  46.34400|    16603334|     472.3461| NA          |
| Mozambique               | Africa    |  2002|  44.02600|    18473780|     633.6179| NA          |
| Mozambique               | Africa    |  2007|  42.08200|    19951656|     823.6856| NA          |
| Myanmar                  | Asia      |  1952|  36.31900|    20092996|     331.0000| NA          |
| Myanmar                  | Asia      |  1957|  41.90500|    21731844|     350.0000| NA          |
| Myanmar                  | Asia      |  1962|  45.10800|    23634436|     388.0000| NA          |
| Myanmar                  | Asia      |  1967|  49.37900|    25870271|     349.0000| NA          |
| Myanmar                  | Asia      |  1972|  53.07000|    28466390|     357.0000| NA          |
| Myanmar                  | Asia      |  1977|  56.05900|    31528087|     371.0000| NA          |
| Myanmar                  | Asia      |  1982|  58.05600|    34680442|     424.0000| NA          |
| Myanmar                  | Asia      |  1987|  58.33900|    38028578|     385.0000| NA          |
| Myanmar                  | Asia      |  1992|  59.32000|    40546538|     347.0000| NA          |
| Myanmar                  | Asia      |  1997|  60.32800|    43247867|     415.0000| NA          |
| Myanmar                  | Asia      |  2002|  59.90800|    45598081|     611.0000| NA          |
| Myanmar                  | Asia      |  2007|  62.06900|    47761980|     944.0000| NA          |
| Namibia                  | Africa    |  1952|  41.72500|      485831|    2423.7804| NA          |
| Namibia                  | Africa    |  1957|  45.22600|      548080|    2621.4481| NA          |
| Namibia                  | Africa    |  1962|  48.38600|      621392|    3173.2156| NA          |
| Namibia                  | Africa    |  1967|  51.15900|      706640|    3793.6948| NA          |
| Namibia                  | Africa    |  1972|  53.86700|      821782|    3746.0809| NA          |
| Namibia                  | Africa    |  1977|  56.43700|      977026|    3876.4860| NA          |
| Namibia                  | Africa    |  1982|  58.96800|     1099010|    4191.1005| NA          |
| Namibia                  | Africa    |  1987|  60.83500|     1278184|    3693.7313| NA          |
| Namibia                  | Africa    |  1992|  61.99900|     1554253|    3804.5380| NA          |
| Namibia                  | Africa    |  1997|  58.90900|     1774766|    3899.5243| NA          |
| Namibia                  | Africa    |  2002|  51.47900|     1972153|    4072.3248| NA          |
| Namibia                  | Africa    |  2007|  52.90600|     2055080|    4811.0604| NA          |
| Nepal                    | Asia      |  1952|  36.15700|     9182536|     545.8657| NA          |
| Nepal                    | Asia      |  1957|  37.68600|     9682338|     597.9364| NA          |
| Nepal                    | Asia      |  1962|  39.39300|    10332057|     652.3969| NA          |
| Nepal                    | Asia      |  1967|  41.47200|    11261690|     676.4422| NA          |
| Nepal                    | Asia      |  1972|  43.97100|    12412593|     674.7881| NA          |
| Nepal                    | Asia      |  1977|  46.74800|    13933198|     694.1124| NA          |
| Nepal                    | Asia      |  1982|  49.59400|    15796314|     718.3731| NA          |
| Nepal                    | Asia      |  1987|  52.53700|    17917180|     775.6325| NA          |
| Nepal                    | Asia      |  1992|  55.72700|    20326209|     897.7404| NA          |
| Nepal                    | Asia      |  1997|  59.42600|    23001113|    1010.8921| NA          |
| Nepal                    | Asia      |  2002|  61.34000|    25873917|    1057.2063| NA          |
| Nepal                    | Asia      |  2007|  63.78500|    28901790|    1091.3598| NA          |
| Netherlands              | Europe    |  1952|  72.13000|    10381988|    8941.5719| NA          |
| Netherlands              | Europe    |  1957|  72.99000|    11026383|   11276.1934| NA          |
| Netherlands              | Europe    |  1962|  73.23000|    11805689|   12790.8496| NA          |
| Netherlands              | Europe    |  1967|  73.82000|    12596822|   15363.2514| NA          |
| Netherlands              | Europe    |  1972|  73.75000|    13329874|   18794.7457| NA          |
| Netherlands              | Europe    |  1977|  75.24000|    13852989|   21209.0592| NA          |
| Netherlands              | Europe    |  1982|  76.05000|    14310401|   21399.4605| NA          |
| Netherlands              | Europe    |  1987|  76.83000|    14665278|   23651.3236| NA          |
| Netherlands              | Europe    |  1992|  77.42000|    15174244|   26790.9496| NA          |
| Netherlands              | Europe    |  1997|  78.03000|    15604464|   30246.1306| NA          |
| Netherlands              | Europe    |  2002|  78.53000|    16122830|   33724.7578| NA          |
| Netherlands              | Europe    |  2007|  79.76200|    16570613|   36797.9333| NA          |
| New Zealand              | Oceania   |  1952|  69.39000|     1994794|   10556.5757| Wellington  |
| New Zealand              | Oceania   |  1957|  70.26000|     2229407|   12247.3953| Wellington  |
| New Zealand              | Oceania   |  1962|  71.24000|     2488550|   13175.6780| Wellington  |
| New Zealand              | Oceania   |  1967|  71.52000|     2728150|   14463.9189| Wellington  |
| New Zealand              | Oceania   |  1972|  71.89000|     2929100|   16046.0373| Wellington  |
| New Zealand              | Oceania   |  1977|  72.22000|     3164900|   16233.7177| Wellington  |
| New Zealand              | Oceania   |  1982|  73.84000|     3210650|   17632.4104| Wellington  |
| New Zealand              | Oceania   |  1987|  74.32000|     3317166|   19007.1913| Wellington  |
| New Zealand              | Oceania   |  1992|  76.33000|     3437674|   18363.3249| Wellington  |
| New Zealand              | Oceania   |  1997|  77.55000|     3676187|   21050.4138| Wellington  |
| New Zealand              | Oceania   |  2002|  79.11000|     3908037|   23189.8014| Wellington  |
| New Zealand              | Oceania   |  2007|  80.20400|     4115771|   25185.0091| Wellington  |
| Nicaragua                | Americas  |  1952|  42.31400|     1165790|    3112.3639| NA          |
| Nicaragua                | Americas  |  1957|  45.43200|     1358828|    3457.4159| NA          |
| Nicaragua                | Americas  |  1962|  48.63200|     1590597|    3634.3644| NA          |
| Nicaragua                | Americas  |  1967|  51.88400|     1865490|    4643.3935| NA          |
| Nicaragua                | Americas  |  1972|  55.15100|     2182908|    4688.5933| NA          |
| Nicaragua                | Americas  |  1977|  57.47000|     2554598|    5486.3711| NA          |
| Nicaragua                | Americas  |  1982|  59.29800|     2979423|    3470.3382| NA          |
| Nicaragua                | Americas  |  1987|  62.00800|     3344353|    2955.9844| NA          |
| Nicaragua                | Americas  |  1992|  65.84300|     4017939|    2170.1517| NA          |
| Nicaragua                | Americas  |  1997|  68.42600|     4609572|    2253.0230| NA          |
| Nicaragua                | Americas  |  2002|  70.83600|     5146848|    2474.5488| NA          |
| Nicaragua                | Americas  |  2007|  72.89900|     5675356|    2749.3210| NA          |
| Niger                    | Africa    |  1952|  37.44400|     3379468|     761.8794| NA          |
| Niger                    | Africa    |  1957|  38.59800|     3692184|     835.5234| NA          |
| Niger                    | Africa    |  1962|  39.48700|     4076008|     997.7661| NA          |
| Niger                    | Africa    |  1967|  40.11800|     4534062|    1054.3849| NA          |
| Niger                    | Africa    |  1972|  40.54600|     5060262|     954.2092| NA          |
| Niger                    | Africa    |  1977|  41.29100|     5682086|     808.8971| NA          |
| Niger                    | Africa    |  1982|  42.59800|     6437188|     909.7221| NA          |
| Niger                    | Africa    |  1987|  44.55500|     7332638|     668.3000| NA          |
| Niger                    | Africa    |  1992|  47.39100|     8392818|     581.1827| NA          |
| Niger                    | Africa    |  1997|  51.31300|     9666252|     580.3052| NA          |
| Niger                    | Africa    |  2002|  54.49600|    11140655|     601.0745| NA          |
| Niger                    | Africa    |  2007|  56.86700|    12894865|     619.6769| NA          |
| Nigeria                  | Africa    |  1952|  36.32400|    33119096|    1077.2819| NA          |
| Nigeria                  | Africa    |  1957|  37.80200|    37173340|    1100.5926| NA          |
| Nigeria                  | Africa    |  1962|  39.36000|    41871351|    1150.9275| NA          |
| Nigeria                  | Africa    |  1967|  41.04000|    47287752|    1014.5141| NA          |
| Nigeria                  | Africa    |  1972|  42.82100|    53740085|    1698.3888| NA          |
| Nigeria                  | Africa    |  1977|  44.51400|    62209173|    1981.9518| NA          |
| Nigeria                  | Africa    |  1982|  45.82600|    73039376|    1576.9738| NA          |
| Nigeria                  | Africa    |  1987|  46.88600|    81551520|    1385.0296| NA          |
| Nigeria                  | Africa    |  1992|  47.47200|    93364244|    1619.8482| NA          |
| Nigeria                  | Africa    |  1997|  47.46400|   106207839|    1624.9413| NA          |
| Nigeria                  | Africa    |  2002|  46.60800|   119901274|    1615.2864| NA          |
| Nigeria                  | Africa    |  2007|  46.85900|   135031164|    2013.9773| NA          |
| Norway                   | Europe    |  1952|  72.67000|     3327728|   10095.4217| Oslo        |
| Norway                   | Europe    |  1957|  73.44000|     3491938|   11653.9730| Oslo        |
| Norway                   | Europe    |  1962|  73.47000|     3638919|   13450.4015| Oslo        |
| Norway                   | Europe    |  1967|  74.08000|     3786019|   16361.8765| Oslo        |
| Norway                   | Europe    |  1972|  74.34000|     3933004|   18965.0555| Oslo        |
| Norway                   | Europe    |  1977|  75.37000|     4043205|   23311.3494| Oslo        |
| Norway                   | Europe    |  1982|  75.97000|     4114787|   26298.6353| Oslo        |
| Norway                   | Europe    |  1987|  75.89000|     4186147|   31540.9748| Oslo        |
| Norway                   | Europe    |  1992|  77.32000|     4286357|   33965.6611| Oslo        |
| Norway                   | Europe    |  1997|  78.32000|     4405672|   41283.1643| Oslo        |
| Norway                   | Europe    |  2002|  79.05000|     4535591|   44683.9753| Oslo        |
| Norway                   | Europe    |  2007|  80.19600|     4627926|   49357.1902| Oslo        |
| Oman                     | Asia      |  1952|  37.57800|      507833|    1828.2303| NA          |
| Oman                     | Asia      |  1957|  40.08000|      561977|    2242.7466| NA          |
| Oman                     | Asia      |  1962|  43.16500|      628164|    2924.6381| NA          |
| Oman                     | Asia      |  1967|  46.98800|      714775|    4720.9427| NA          |
| Oman                     | Asia      |  1972|  52.14300|      829050|   10618.0385| NA          |
| Oman                     | Asia      |  1977|  57.36700|     1004533|   11848.3439| NA          |
| Oman                     | Asia      |  1982|  62.72800|     1301048|   12954.7910| NA          |
| Oman                     | Asia      |  1987|  67.73400|     1593882|   18115.2231| NA          |
| Oman                     | Asia      |  1992|  71.19700|     1915208|   18616.7069| NA          |
| Oman                     | Asia      |  1997|  72.49900|     2283635|   19702.0558| NA          |
| Oman                     | Asia      |  2002|  74.19300|     2713462|   19774.8369| NA          |
| Oman                     | Asia      |  2007|  75.64000|     3204897|   22316.1929| NA          |
| Pakistan                 | Asia      |  1952|  43.43600|    41346560|     684.5971| NA          |
| Pakistan                 | Asia      |  1957|  45.55700|    46679944|     747.0835| NA          |
| Pakistan                 | Asia      |  1962|  47.67000|    53100671|     803.3427| NA          |
| Pakistan                 | Asia      |  1967|  49.80000|    60641899|     942.4083| NA          |
| Pakistan                 | Asia      |  1972|  51.92900|    69325921|    1049.9390| NA          |
| Pakistan                 | Asia      |  1977|  54.04300|    78152686|    1175.9212| NA          |
| Pakistan                 | Asia      |  1982|  56.15800|    91462088|    1443.4298| NA          |
| Pakistan                 | Asia      |  1987|  58.24500|   105186881|    1704.6866| NA          |
| Pakistan                 | Asia      |  1992|  60.83800|   120065004|    1971.8295| NA          |
| Pakistan                 | Asia      |  1997|  61.81800|   135564834|    2049.3505| NA          |
| Pakistan                 | Asia      |  2002|  63.61000|   153403524|    2092.7124| NA          |
| Pakistan                 | Asia      |  2007|  65.48300|   169270617|    2605.9476| NA          |
| Panama                   | Americas  |  1952|  55.19100|      940080|    2480.3803| NA          |
| Panama                   | Americas  |  1957|  59.20100|     1063506|    2961.8009| NA          |
| Panama                   | Americas  |  1962|  61.81700|     1215725|    3536.5403| NA          |
| Panama                   | Americas  |  1967|  64.07100|     1405486|    4421.0091| NA          |
| Panama                   | Americas  |  1972|  66.21600|     1616384|    5364.2497| NA          |
| Panama                   | Americas  |  1977|  68.68100|     1839782|    5351.9121| NA          |
| Panama                   | Americas  |  1982|  70.47200|     2036305|    7009.6016| NA          |
| Panama                   | Americas  |  1987|  71.52300|     2253639|    7034.7792| NA          |
| Panama                   | Americas  |  1992|  72.46200|     2484997|    6618.7431| NA          |
| Panama                   | Americas  |  1997|  73.73800|     2734531|    7113.6923| NA          |
| Panama                   | Americas  |  2002|  74.71200|     2990875|    7356.0319| NA          |
| Panama                   | Americas  |  2007|  75.53700|     3242173|    9809.1856| NA          |
| Paraguay                 | Americas  |  1952|  62.64900|     1555876|    1952.3087| NA          |
| Paraguay                 | Americas  |  1957|  63.19600|     1770902|    2046.1547| NA          |
| Paraguay                 | Americas  |  1962|  64.36100|     2009813|    2148.0271| NA          |
| Paraguay                 | Americas  |  1967|  64.95100|     2287985|    2299.3763| NA          |
| Paraguay                 | Americas  |  1972|  65.81500|     2614104|    2523.3380| NA          |
| Paraguay                 | Americas  |  1977|  66.35300|     2984494|    3248.3733| NA          |
| Paraguay                 | Americas  |  1982|  66.87400|     3366439|    4258.5036| NA          |
| Paraguay                 | Americas  |  1987|  67.37800|     3886512|    3998.8757| NA          |
| Paraguay                 | Americas  |  1992|  68.22500|     4483945|    4196.4111| NA          |
| Paraguay                 | Americas  |  1997|  69.40000|     5154123|    4247.4003| NA          |
| Paraguay                 | Americas  |  2002|  70.75500|     5884491|    3783.6742| NA          |
| Paraguay                 | Americas  |  2007|  71.75200|     6667147|    4172.8385| NA          |
| Peru                     | Americas  |  1952|  43.90200|     8025700|    3758.5234| NA          |
| Peru                     | Americas  |  1957|  46.26300|     9146100|    4245.2567| NA          |
| Peru                     | Americas  |  1962|  49.09600|    10516500|    4957.0380| NA          |
| Peru                     | Americas  |  1967|  51.44500|    12132200|    5788.0933| NA          |
| Peru                     | Americas  |  1972|  55.44800|    13954700|    5937.8273| NA          |
| Peru                     | Americas  |  1977|  58.44700|    15990099|    6281.2909| NA          |
| Peru                     | Americas  |  1982|  61.40600|    18125129|    6434.5018| NA          |
| Peru                     | Americas  |  1987|  64.13400|    20195924|    6360.9434| NA          |
| Peru                     | Americas  |  1992|  66.45800|    22430449|    4446.3809| NA          |
| Peru                     | Americas  |  1997|  68.38600|    24748122|    5838.3477| NA          |
| Peru                     | Americas  |  2002|  69.90600|    26769436|    5909.0201| NA          |
| Peru                     | Americas  |  2007|  71.42100|    28674757|    7408.9056| NA          |
| Philippines              | Asia      |  1952|  47.75200|    22438691|    1272.8810| NA          |
| Philippines              | Asia      |  1957|  51.33400|    26072194|    1547.9448| NA          |
| Philippines              | Asia      |  1962|  54.75700|    30325264|    1649.5522| NA          |
| Philippines              | Asia      |  1967|  56.39300|    35356600|    1814.1274| NA          |
| Philippines              | Asia      |  1972|  58.06500|    40850141|    1989.3741| NA          |
| Philippines              | Asia      |  1977|  60.06000|    46850962|    2373.2043| NA          |
| Philippines              | Asia      |  1982|  62.08200|    53456774|    2603.2738| NA          |
| Philippines              | Asia      |  1987|  64.15100|    60017788|    2189.6350| NA          |
| Philippines              | Asia      |  1992|  66.45800|    67185766|    2279.3240| NA          |
| Philippines              | Asia      |  1997|  68.56400|    75012988|    2536.5349| NA          |
| Philippines              | Asia      |  2002|  70.30300|    82995088|    2650.9211| NA          |
| Philippines              | Asia      |  2007|  71.68800|    91077287|    3190.4810| NA          |
| Poland                   | Europe    |  1952|  61.31000|    25730551|    4029.3297| NA          |
| Poland                   | Europe    |  1957|  65.77000|    28235346|    4734.2530| NA          |
| Poland                   | Europe    |  1962|  67.64000|    30329617|    5338.7521| NA          |
| Poland                   | Europe    |  1967|  69.61000|    31785378|    6557.1528| NA          |
| Poland                   | Europe    |  1972|  70.85000|    33039545|    8006.5070| NA          |
| Poland                   | Europe    |  1977|  70.67000|    34621254|    9508.1415| NA          |
| Poland                   | Europe    |  1982|  71.32000|    36227381|    8451.5310| NA          |
| Poland                   | Europe    |  1987|  70.98000|    37740710|    9082.3512| NA          |
| Poland                   | Europe    |  1992|  70.99000|    38370697|    7738.8812| NA          |
| Poland                   | Europe    |  1997|  72.75000|    38654957|   10159.5837| NA          |
| Poland                   | Europe    |  2002|  74.67000|    38625976|   12002.2391| NA          |
| Poland                   | Europe    |  2007|  75.56300|    38518241|   15389.9247| NA          |
| Portugal                 | Europe    |  1952|  59.82000|     8526050|    3068.3199| NA          |
| Portugal                 | Europe    |  1957|  61.51000|     8817650|    3774.5717| NA          |
| Portugal                 | Europe    |  1962|  64.39000|     9019800|    4727.9549| NA          |
| Portugal                 | Europe    |  1967|  66.60000|     9103000|    6361.5180| NA          |
| Portugal                 | Europe    |  1972|  69.26000|     8970450|    9022.2474| NA          |
| Portugal                 | Europe    |  1977|  70.41000|     9662600|   10172.4857| NA          |
| Portugal                 | Europe    |  1982|  72.77000|     9859650|   11753.8429| NA          |
| Portugal                 | Europe    |  1987|  74.06000|     9915289|   13039.3088| NA          |
| Portugal                 | Europe    |  1992|  74.86000|     9927680|   16207.2666| NA          |
| Portugal                 | Europe    |  1997|  75.97000|    10156415|   17641.0316| NA          |
| Portugal                 | Europe    |  2002|  77.29000|    10433867|   19970.9079| NA          |
| Portugal                 | Europe    |  2007|  78.09800|    10642836|   20509.6478| NA          |
| Puerto Rico              | Americas  |  1952|  64.28000|     2227000|    3081.9598| NA          |
| Puerto Rico              | Americas  |  1957|  68.54000|     2260000|    3907.1562| NA          |
| Puerto Rico              | Americas  |  1962|  69.62000|     2448046|    5108.3446| NA          |
| Puerto Rico              | Americas  |  1967|  71.10000|     2648961|    6929.2777| NA          |
| Puerto Rico              | Americas  |  1972|  72.16000|     2847132|    9123.0417| NA          |
| Puerto Rico              | Americas  |  1977|  73.44000|     3080828|    9770.5249| NA          |
| Puerto Rico              | Americas  |  1982|  73.75000|     3279001|   10330.9891| NA          |
| Puerto Rico              | Americas  |  1987|  74.63000|     3444468|   12281.3419| NA          |
| Puerto Rico              | Americas  |  1992|  73.91100|     3585176|   14641.5871| NA          |
| Puerto Rico              | Americas  |  1997|  74.91700|     3759430|   16999.4333| NA          |
| Puerto Rico              | Americas  |  2002|  77.77800|     3859606|   18855.6062| NA          |
| Puerto Rico              | Americas  |  2007|  78.74600|     3942491|   19328.7090| NA          |
| Reunion                  | Africa    |  1952|  52.72400|      257700|    2718.8853| NA          |
| Reunion                  | Africa    |  1957|  55.09000|      308700|    2769.4518| NA          |
| Reunion                  | Africa    |  1962|  57.66600|      358900|    3173.7233| NA          |
| Reunion                  | Africa    |  1967|  60.54200|      414024|    4021.1757| NA          |
| Reunion                  | Africa    |  1972|  64.27400|      461633|    5047.6586| NA          |
| Reunion                  | Africa    |  1977|  67.06400|      492095|    4319.8041| NA          |
| Reunion                  | Africa    |  1982|  69.88500|      517810|    5267.2194| NA          |
| Reunion                  | Africa    |  1987|  71.91300|      562035|    5303.3775| NA          |
| Reunion                  | Africa    |  1992|  73.61500|      622191|    6101.2558| NA          |
| Reunion                  | Africa    |  1997|  74.77200|      684810|    6071.9414| NA          |
| Reunion                  | Africa    |  2002|  75.74400|      743981|    6316.1652| NA          |
| Reunion                  | Africa    |  2007|  76.44200|      798094|    7670.1226| NA          |
| Romania                  | Europe    |  1952|  61.05000|    16630000|    3144.6132| NA          |
| Romania                  | Europe    |  1957|  64.10000|    17829327|    3943.3702| NA          |
| Romania                  | Europe    |  1962|  66.80000|    18680721|    4734.9976| NA          |
| Romania                  | Europe    |  1967|  66.80000|    19284814|    6470.8665| NA          |
| Romania                  | Europe    |  1972|  69.21000|    20662648|    8011.4144| NA          |
| Romania                  | Europe    |  1977|  69.46000|    21658597|    9356.3972| NA          |
| Romania                  | Europe    |  1982|  69.66000|    22356726|    9605.3141| NA          |
| Romania                  | Europe    |  1987|  69.53000|    22686371|    9696.2733| NA          |
| Romania                  | Europe    |  1992|  69.36000|    22797027|    6598.4099| NA          |
| Romania                  | Europe    |  1997|  69.72000|    22562458|    7346.5476| NA          |
| Romania                  | Europe    |  2002|  71.32200|    22404337|    7885.3601| NA          |
| Romania                  | Europe    |  2007|  72.47600|    22276056|   10808.4756| NA          |
| Rwanda                   | Africa    |  1952|  40.00000|     2534927|     493.3239| NA          |
| Rwanda                   | Africa    |  1957|  41.50000|     2822082|     540.2894| NA          |
| Rwanda                   | Africa    |  1962|  43.00000|     3051242|     597.4731| NA          |
| Rwanda                   | Africa    |  1967|  44.10000|     3451079|     510.9637| NA          |
| Rwanda                   | Africa    |  1972|  44.60000|     3992121|     590.5807| NA          |
| Rwanda                   | Africa    |  1977|  45.00000|     4657072|     670.0806| NA          |
| Rwanda                   | Africa    |  1982|  46.21800|     5507565|     881.5706| NA          |
| Rwanda                   | Africa    |  1987|  44.02000|     6349365|     847.9912| NA          |
| Rwanda                   | Africa    |  1992|  23.59900|     7290203|     737.0686| NA          |
| Rwanda                   | Africa    |  1997|  36.08700|     7212583|     589.9445| NA          |
| Rwanda                   | Africa    |  2002|  43.41300|     7852401|     785.6538| NA          |
| Rwanda                   | Africa    |  2007|  46.24200|     8860588|     863.0885| NA          |
| Sao Tome and Principe    | Africa    |  1952|  46.47100|       60011|     879.5836| NA          |
| Sao Tome and Principe    | Africa    |  1957|  48.94500|       61325|     860.7369| NA          |
| Sao Tome and Principe    | Africa    |  1962|  51.89300|       65345|    1071.5511| NA          |
| Sao Tome and Principe    | Africa    |  1967|  54.42500|       70787|    1384.8406| NA          |
| Sao Tome and Principe    | Africa    |  1972|  56.48000|       76595|    1532.9853| NA          |
| Sao Tome and Principe    | Africa    |  1977|  58.55000|       86796|    1737.5617| NA          |
| Sao Tome and Principe    | Africa    |  1982|  60.35100|       98593|    1890.2181| NA          |
| Sao Tome and Principe    | Africa    |  1987|  61.72800|      110812|    1516.5255| NA          |
| Sao Tome and Principe    | Africa    |  1992|  62.74200|      125911|    1428.7778| NA          |
| Sao Tome and Principe    | Africa    |  1997|  63.30600|      145608|    1339.0760| NA          |
| Sao Tome and Principe    | Africa    |  2002|  64.33700|      170372|    1353.0924| NA          |
| Sao Tome and Principe    | Africa    |  2007|  65.52800|      199579|    1598.4351| NA          |
| Saudi Arabia             | Asia      |  1952|  39.87500|     4005677|    6459.5548| NA          |
| Saudi Arabia             | Asia      |  1957|  42.86800|     4419650|    8157.5912| NA          |
| Saudi Arabia             | Asia      |  1962|  45.91400|     4943029|   11626.4197| NA          |
| Saudi Arabia             | Asia      |  1967|  49.90100|     5618198|   16903.0489| NA          |
| Saudi Arabia             | Asia      |  1972|  53.88600|     6472756|   24837.4287| NA          |
| Saudi Arabia             | Asia      |  1977|  58.69000|     8128505|   34167.7626| NA          |
| Saudi Arabia             | Asia      |  1982|  63.01200|    11254672|   33693.1753| NA          |
| Saudi Arabia             | Asia      |  1987|  66.29500|    14619745|   21198.2614| NA          |
| Saudi Arabia             | Asia      |  1992|  68.76800|    16945857|   24841.6178| NA          |
| Saudi Arabia             | Asia      |  1997|  70.53300|    21229759|   20586.6902| NA          |
| Saudi Arabia             | Asia      |  2002|  71.62600|    24501530|   19014.5412| NA          |
| Saudi Arabia             | Asia      |  2007|  72.77700|    27601038|   21654.8319| NA          |
| Senegal                  | Africa    |  1952|  37.27800|     2755589|    1450.3570| NA          |
| Senegal                  | Africa    |  1957|  39.32900|     3054547|    1567.6530| NA          |
| Senegal                  | Africa    |  1962|  41.45400|     3430243|    1654.9887| NA          |
| Senegal                  | Africa    |  1967|  43.56300|     3965841|    1612.4046| NA          |
| Senegal                  | Africa    |  1972|  45.81500|     4588696|    1597.7121| NA          |
| Senegal                  | Africa    |  1977|  48.87900|     5260855|    1561.7691| NA          |
| Senegal                  | Africa    |  1982|  52.37900|     6147783|    1518.4800| NA          |
| Senegal                  | Africa    |  1987|  55.76900|     7171347|    1441.7207| NA          |
| Senegal                  | Africa    |  1992|  58.19600|     8307920|    1367.8994| NA          |
| Senegal                  | Africa    |  1997|  60.18700|     9535314|    1392.3683| NA          |
| Senegal                  | Africa    |  2002|  61.60000|    10870037|    1519.6353| NA          |
| Senegal                  | Africa    |  2007|  63.06200|    12267493|    1712.4721| NA          |
| Serbia                   | Europe    |  1952|  57.99600|     6860147|    3581.4594| NA          |
| Serbia                   | Europe    |  1957|  61.68500|     7271135|    4981.0909| NA          |
| Serbia                   | Europe    |  1962|  64.53100|     7616060|    6289.6292| NA          |
| Serbia                   | Europe    |  1967|  66.91400|     7971222|    7991.7071| NA          |
| Serbia                   | Europe    |  1972|  68.70000|     8313288|   10522.0675| NA          |
| Serbia                   | Europe    |  1977|  70.30000|     8686367|   12980.6696| NA          |
| Serbia                   | Europe    |  1982|  70.16200|     9032824|   15181.0927| NA          |
| Serbia                   | Europe    |  1987|  71.21800|     9230783|   15870.8785| NA          |
| Serbia                   | Europe    |  1992|  71.65900|     9826397|    9325.0682| NA          |
| Serbia                   | Europe    |  1997|  72.23200|    10336594|    7914.3203| NA          |
| Serbia                   | Europe    |  2002|  73.21300|    10111559|    7236.0753| NA          |
| Serbia                   | Europe    |  2007|  74.00200|    10150265|    9786.5347| NA          |
| Sierra Leone             | Africa    |  1952|  30.33100|     2143249|     879.7877| NA          |
| Sierra Leone             | Africa    |  1957|  31.57000|     2295678|    1004.4844| NA          |
| Sierra Leone             | Africa    |  1962|  32.76700|     2467895|    1116.6399| NA          |
| Sierra Leone             | Africa    |  1967|  34.11300|     2662190|    1206.0435| NA          |
| Sierra Leone             | Africa    |  1972|  35.40000|     2879013|    1353.7598| NA          |
| Sierra Leone             | Africa    |  1977|  36.78800|     3140897|    1348.2852| NA          |
| Sierra Leone             | Africa    |  1982|  38.44500|     3464522|    1465.0108| NA          |
| Sierra Leone             | Africa    |  1987|  40.00600|     3868905|    1294.4478| NA          |
| Sierra Leone             | Africa    |  1992|  38.33300|     4260884|    1068.6963| NA          |
| Sierra Leone             | Africa    |  1997|  39.89700|     4578212|     574.6482| NA          |
| Sierra Leone             | Africa    |  2002|  41.01200|     5359092|     699.4897| NA          |
| Sierra Leone             | Africa    |  2007|  42.56800|     6144562|     862.5408| NA          |
| Singapore                | Asia      |  1952|  60.39600|     1127000|    2315.1382| NA          |
| Singapore                | Asia      |  1957|  63.17900|     1445929|    2843.1044| NA          |
| Singapore                | Asia      |  1962|  65.79800|     1750200|    3674.7356| NA          |
| Singapore                | Asia      |  1967|  67.94600|     1977600|    4977.4185| NA          |
| Singapore                | Asia      |  1972|  69.52100|     2152400|    8597.7562| NA          |
| Singapore                | Asia      |  1977|  70.79500|     2325300|   11210.0895| NA          |
| Singapore                | Asia      |  1982|  71.76000|     2651869|   15169.1611| NA          |
| Singapore                | Asia      |  1987|  73.56000|     2794552|   18861.5308| NA          |
| Singapore                | Asia      |  1992|  75.78800|     3235865|   24769.8912| NA          |
| Singapore                | Asia      |  1997|  77.15800|     3802309|   33519.4766| NA          |
| Singapore                | Asia      |  2002|  78.77000|     4197776|   36023.1054| NA          |
| Singapore                | Asia      |  2007|  79.97200|     4553009|   47143.1796| NA          |
| Slovak Republic          | Europe    |  1952|  64.36000|     3558137|    5074.6591| NA          |
| Slovak Republic          | Europe    |  1957|  67.45000|     3844277|    6093.2630| NA          |
| Slovak Republic          | Europe    |  1962|  70.33000|     4237384|    7481.1076| NA          |
| Slovak Republic          | Europe    |  1967|  70.98000|     4442238|    8412.9024| NA          |
| Slovak Republic          | Europe    |  1972|  70.35000|     4593433|    9674.1676| NA          |
| Slovak Republic          | Europe    |  1977|  70.45000|     4827803|   10922.6640| NA          |
| Slovak Republic          | Europe    |  1982|  70.80000|     5048043|   11348.5459| NA          |
| Slovak Republic          | Europe    |  1987|  71.08000|     5199318|   12037.2676| NA          |
| Slovak Republic          | Europe    |  1992|  71.38000|     5302888|    9498.4677| NA          |
| Slovak Republic          | Europe    |  1997|  72.71000|     5383010|   12126.2306| NA          |
| Slovak Republic          | Europe    |  2002|  73.80000|     5410052|   13638.7784| NA          |
| Slovak Republic          | Europe    |  2007|  74.66300|     5447502|   18678.3144| NA          |
| Slovenia                 | Europe    |  1952|  65.57000|     1489518|    4215.0417| NA          |
| Slovenia                 | Europe    |  1957|  67.85000|     1533070|    5862.2766| NA          |
| Slovenia                 | Europe    |  1962|  69.15000|     1582962|    7402.3034| NA          |
| Slovenia                 | Europe    |  1967|  69.18000|     1646912|    9405.4894| NA          |
| Slovenia                 | Europe    |  1972|  69.82000|     1694510|   12383.4862| NA          |
| Slovenia                 | Europe    |  1977|  70.97000|     1746919|   15277.0302| NA          |
| Slovenia                 | Europe    |  1982|  71.06300|     1861252|   17866.7218| NA          |
| Slovenia                 | Europe    |  1987|  72.25000|     1945870|   18678.5349| NA          |
| Slovenia                 | Europe    |  1992|  73.64000|     1999210|   14214.7168| NA          |
| Slovenia                 | Europe    |  1997|  75.13000|     2011612|   17161.1073| NA          |
| Slovenia                 | Europe    |  2002|  76.66000|     2011497|   20660.0194| NA          |
| Slovenia                 | Europe    |  2007|  77.92600|     2009245|   25768.2576| NA          |
| Somalia                  | Africa    |  1952|  32.97800|     2526994|    1135.7498| Mogadishu   |
| Somalia                  | Africa    |  1957|  34.97700|     2780415|    1258.1474| Mogadishu   |
| Somalia                  | Africa    |  1962|  36.98100|     3080153|    1369.4883| Mogadishu   |
| Somalia                  | Africa    |  1967|  38.97700|     3428839|    1284.7332| Mogadishu   |
| Somalia                  | Africa    |  1972|  40.97300|     3840161|    1254.5761| Mogadishu   |
| Somalia                  | Africa    |  1977|  41.97400|     4353666|    1450.9925| Mogadishu   |
| Somalia                  | Africa    |  1982|  42.95500|     5828892|    1176.8070| Mogadishu   |
| Somalia                  | Africa    |  1987|  44.50100|     6921858|    1093.2450| Mogadishu   |
| Somalia                  | Africa    |  1992|  39.65800|     6099799|     926.9603| Mogadishu   |
| Somalia                  | Africa    |  1997|  43.79500|     6633514|     930.5964| Mogadishu   |
| Somalia                  | Africa    |  2002|  45.93600|     7753310|     882.0818| Mogadishu   |
| Somalia                  | Africa    |  2007|  48.15900|     9118773|     926.1411| Mogadishu   |
| South Africa             | Africa    |  1952|  45.00900|    14264935|    4725.2955| NA          |
| South Africa             | Africa    |  1957|  47.98500|    16151549|    5487.1042| NA          |
| South Africa             | Africa    |  1962|  49.95100|    18356657|    5768.7297| NA          |
| South Africa             | Africa    |  1967|  51.92700|    20997321|    7114.4780| NA          |
| South Africa             | Africa    |  1972|  53.69600|    23935810|    7765.9626| NA          |
| South Africa             | Africa    |  1977|  55.52700|    27129932|    8028.6514| NA          |
| South Africa             | Africa    |  1982|  58.16100|    31140029|    8568.2662| NA          |
| South Africa             | Africa    |  1987|  60.83400|    35933379|    7825.8234| NA          |
| South Africa             | Africa    |  1992|  61.88800|    39964159|    7225.0693| NA          |
| South Africa             | Africa    |  1997|  60.23600|    42835005|    7479.1882| NA          |
| South Africa             | Africa    |  2002|  53.36500|    44433622|    7710.9464| NA          |
| South Africa             | Africa    |  2007|  49.33900|    43997828|    9269.6578| NA          |
| Spain                    | Europe    |  1952|  64.94000|    28549870|    3834.0347| NA          |
| Spain                    | Europe    |  1957|  66.66000|    29841614|    4564.8024| NA          |
| Spain                    | Europe    |  1962|  69.69000|    31158061|    5693.8439| NA          |
| Spain                    | Europe    |  1967|  71.44000|    32850275|    7993.5123| NA          |
| Spain                    | Europe    |  1972|  73.06000|    34513161|   10638.7513| NA          |
| Spain                    | Europe    |  1977|  74.39000|    36439000|   13236.9212| NA          |
| Spain                    | Europe    |  1982|  76.30000|    37983310|   13926.1700| NA          |
| Spain                    | Europe    |  1987|  76.90000|    38880702|   15764.9831| NA          |
| Spain                    | Europe    |  1992|  77.57000|    39549438|   18603.0645| NA          |
| Spain                    | Europe    |  1997|  78.77000|    39855442|   20445.2990| NA          |
| Spain                    | Europe    |  2002|  79.78000|    40152517|   24835.4717| NA          |
| Spain                    | Europe    |  2007|  80.94100|    40448191|   28821.0637| NA          |
| Sri Lanka                | Asia      |  1952|  57.59300|     7982342|    1083.5320| NA          |
| Sri Lanka                | Asia      |  1957|  61.45600|     9128546|    1072.5466| NA          |
| Sri Lanka                | Asia      |  1962|  62.19200|    10421936|    1074.4720| NA          |
| Sri Lanka                | Asia      |  1967|  64.26600|    11737396|    1135.5143| NA          |
| Sri Lanka                | Asia      |  1972|  65.04200|    13016733|    1213.3955| NA          |
| Sri Lanka                | Asia      |  1977|  65.94900|    14116836|    1348.7757| NA          |
| Sri Lanka                | Asia      |  1982|  68.75700|    15410151|    1648.0798| NA          |
| Sri Lanka                | Asia      |  1987|  69.01100|    16495304|    1876.7668| NA          |
| Sri Lanka                | Asia      |  1992|  70.37900|    17587060|    2153.7392| NA          |
| Sri Lanka                | Asia      |  1997|  70.45700|    18698655|    2664.4773| NA          |
| Sri Lanka                | Asia      |  2002|  70.81500|    19576783|    3015.3788| NA          |
| Sri Lanka                | Asia      |  2007|  72.39600|    20378239|    3970.0954| NA          |
| Sudan                    | Africa    |  1952|  38.63500|     8504667|    1615.9911| NA          |
| Sudan                    | Africa    |  1957|  39.62400|     9753392|    1770.3371| NA          |
| Sudan                    | Africa    |  1962|  40.87000|    11183227|    1959.5938| NA          |
| Sudan                    | Africa    |  1967|  42.85800|    12716129|    1687.9976| NA          |
| Sudan                    | Africa    |  1972|  45.08300|    14597019|    1659.6528| NA          |
| Sudan                    | Africa    |  1977|  47.80000|    17104986|    2202.9884| NA          |
| Sudan                    | Africa    |  1982|  50.33800|    20367053|    1895.5441| NA          |
| Sudan                    | Africa    |  1987|  51.74400|    24725960|    1507.8192| NA          |
| Sudan                    | Africa    |  1992|  53.55600|    28227588|    1492.1970| NA          |
| Sudan                    | Africa    |  1997|  55.37300|    32160729|    1632.2108| NA          |
| Sudan                    | Africa    |  2002|  56.36900|    37090298|    1993.3983| NA          |
| Sudan                    | Africa    |  2007|  58.55600|    42292929|    2602.3950| NA          |
| Swaziland                | Africa    |  1952|  41.40700|      290243|    1148.3766| NA          |
| Swaziland                | Africa    |  1957|  43.42400|      326741|    1244.7084| NA          |
| Swaziland                | Africa    |  1962|  44.99200|      370006|    1856.1821| NA          |
| Swaziland                | Africa    |  1967|  46.63300|      420690|    2613.1017| NA          |
| Swaziland                | Africa    |  1972|  49.55200|      480105|    3364.8366| NA          |
| Swaziland                | Africa    |  1977|  52.53700|      551425|    3781.4106| NA          |
| Swaziland                | Africa    |  1982|  55.56100|      649901|    3895.3840| NA          |
| Swaziland                | Africa    |  1987|  57.67800|      779348|    3984.8398| NA          |
| Swaziland                | Africa    |  1992|  58.47400|      962344|    3553.0224| NA          |
| Swaziland                | Africa    |  1997|  54.28900|     1054486|    3876.7685| NA          |
| Swaziland                | Africa    |  2002|  43.86900|     1130269|    4128.1169| NA          |
| Swaziland                | Africa    |  2007|  39.61300|     1133066|    4513.4806| NA          |
| Sweden                   | Europe    |  1952|  71.86000|     7124673|    8527.8447| NA          |
| Sweden                   | Europe    |  1957|  72.49000|     7363802|    9911.8782| NA          |
| Sweden                   | Europe    |  1962|  73.37000|     7561588|   12329.4419| NA          |
| Sweden                   | Europe    |  1967|  74.16000|     7867931|   15258.2970| NA          |
| Sweden                   | Europe    |  1972|  74.72000|     8122293|   17832.0246| NA          |
| Sweden                   | Europe    |  1977|  75.44000|     8251648|   18855.7252| NA          |
| Sweden                   | Europe    |  1982|  76.42000|     8325260|   20667.3812| NA          |
| Sweden                   | Europe    |  1987|  77.19000|     8421403|   23586.9293| NA          |
| Sweden                   | Europe    |  1992|  78.16000|     8718867|   23880.0168| NA          |
| Sweden                   | Europe    |  1997|  79.39000|     8897619|   25266.5950| NA          |
| Sweden                   | Europe    |  2002|  80.04000|     8954175|   29341.6309| NA          |
| Sweden                   | Europe    |  2007|  80.88400|     9031088|   33859.7484| NA          |
| Switzerland              | Europe    |  1952|  69.62000|     4815000|   14734.2327| NA          |
| Switzerland              | Europe    |  1957|  70.56000|     5126000|   17909.4897| NA          |
| Switzerland              | Europe    |  1962|  71.32000|     5666000|   20431.0927| NA          |
| Switzerland              | Europe    |  1967|  72.77000|     6063000|   22966.1443| NA          |
| Switzerland              | Europe    |  1972|  73.78000|     6401400|   27195.1130| NA          |
| Switzerland              | Europe    |  1977|  75.39000|     6316424|   26982.2905| NA          |
| Switzerland              | Europe    |  1982|  76.21000|     6468126|   28397.7151| NA          |
| Switzerland              | Europe    |  1987|  77.41000|     6649942|   30281.7046| NA          |
| Switzerland              | Europe    |  1992|  78.03000|     6995447|   31871.5303| NA          |
| Switzerland              | Europe    |  1997|  79.37000|     7193761|   32135.3230| NA          |
| Switzerland              | Europe    |  2002|  80.62000|     7361757|   34480.9577| NA          |
| Switzerland              | Europe    |  2007|  81.70100|     7554661|   37506.4191| NA          |
| Syria                    | Asia      |  1952|  45.88300|     3661549|    1643.4854| NA          |
| Syria                    | Asia      |  1957|  48.28400|     4149908|    2117.2349| NA          |
| Syria                    | Asia      |  1962|  50.30500|     4834621|    2193.0371| NA          |
| Syria                    | Asia      |  1967|  53.65500|     5680812|    1881.9236| NA          |
| Syria                    | Asia      |  1972|  57.29600|     6701172|    2571.4230| NA          |
| Syria                    | Asia      |  1977|  61.19500|     7932503|    3195.4846| NA          |
| Syria                    | Asia      |  1982|  64.59000|     9410494|    3761.8377| NA          |
| Syria                    | Asia      |  1987|  66.97400|    11242847|    3116.7743| NA          |
| Syria                    | Asia      |  1992|  69.24900|    13219062|    3340.5428| NA          |
| Syria                    | Asia      |  1997|  71.52700|    15081016|    4014.2390| NA          |
| Syria                    | Asia      |  2002|  73.05300|    17155814|    4090.9253| NA          |
| Syria                    | Asia      |  2007|  74.14300|    19314747|    4184.5481| NA          |
| Taiwan                   | Asia      |  1952|  58.50000|     8550362|    1206.9479| NA          |
| Taiwan                   | Asia      |  1957|  62.40000|    10164215|    1507.8613| NA          |
| Taiwan                   | Asia      |  1962|  65.20000|    11918938|    1822.8790| NA          |
| Taiwan                   | Asia      |  1967|  67.50000|    13648692|    2643.8587| NA          |
| Taiwan                   | Asia      |  1972|  69.39000|    15226039|    4062.5239| NA          |
| Taiwan                   | Asia      |  1977|  70.59000|    16785196|    5596.5198| NA          |
| Taiwan                   | Asia      |  1982|  72.16000|    18501390|    7426.3548| NA          |
| Taiwan                   | Asia      |  1987|  73.40000|    19757799|   11054.5618| NA          |
| Taiwan                   | Asia      |  1992|  74.26000|    20686918|   15215.6579| NA          |
| Taiwan                   | Asia      |  1997|  75.25000|    21628605|   20206.8210| NA          |
| Taiwan                   | Asia      |  2002|  76.99000|    22454239|   23235.4233| NA          |
| Taiwan                   | Asia      |  2007|  78.40000|    23174294|   28718.2768| NA          |
| Tanzania                 | Africa    |  1952|  41.21500|     8322925|     716.6501| NA          |
| Tanzania                 | Africa    |  1957|  42.97400|     9452826|     698.5356| NA          |
| Tanzania                 | Africa    |  1962|  44.24600|    10863958|     722.0038| NA          |
| Tanzania                 | Africa    |  1967|  45.75700|    12607312|     848.2187| NA          |
| Tanzania                 | Africa    |  1972|  47.62000|    14706593|     915.9851| NA          |
| Tanzania                 | Africa    |  1977|  49.91900|    17129565|     962.4923| NA          |
| Tanzania                 | Africa    |  1982|  50.60800|    19844382|     874.2426| NA          |
| Tanzania                 | Africa    |  1987|  51.53500|    23040630|     831.8221| NA          |
| Tanzania                 | Africa    |  1992|  50.44000|    26605473|     825.6825| NA          |
| Tanzania                 | Africa    |  1997|  48.46600|    30686889|     789.1862| NA          |
| Tanzania                 | Africa    |  2002|  49.65100|    34593779|     899.0742| NA          |
| Tanzania                 | Africa    |  2007|  52.51700|    38139640|    1107.4822| NA          |
| Thailand                 | Asia      |  1952|  50.84800|    21289402|     757.7974| Bangkok     |
| Thailand                 | Asia      |  1957|  53.63000|    25041917|     793.5774| Bangkok     |
| Thailand                 | Asia      |  1962|  56.06100|    29263397|    1002.1992| Bangkok     |
| Thailand                 | Asia      |  1967|  58.28500|    34024249|    1295.4607| Bangkok     |
| Thailand                 | Asia      |  1972|  60.40500|    39276153|    1524.3589| Bangkok     |
| Thailand                 | Asia      |  1977|  62.49400|    44148285|    1961.2246| Bangkok     |
| Thailand                 | Asia      |  1982|  64.59700|    48827160|    2393.2198| Bangkok     |
| Thailand                 | Asia      |  1987|  66.08400|    52910342|    2982.6538| Bangkok     |
| Thailand                 | Asia      |  1992|  67.29800|    56667095|    4616.8965| Bangkok     |
| Thailand                 | Asia      |  1997|  67.52100|    60216677|    5852.6255| Bangkok     |
| Thailand                 | Asia      |  2002|  68.56400|    62806748|    5913.1875| Bangkok     |
| Thailand                 | Asia      |  2007|  70.61600|    65068149|    7458.3963| Bangkok     |
| Togo                     | Africa    |  1952|  38.59600|     1219113|     859.8087| NA          |
| Togo                     | Africa    |  1957|  41.20800|     1357445|     925.9083| NA          |
| Togo                     | Africa    |  1962|  43.92200|     1528098|    1067.5348| NA          |
| Togo                     | Africa    |  1967|  46.76900|     1735550|    1477.5968| NA          |
| Togo                     | Africa    |  1972|  49.75900|     2056351|    1649.6602| NA          |
| Togo                     | Africa    |  1977|  52.88700|     2308582|    1532.7770| NA          |
| Togo                     | Africa    |  1982|  55.47100|     2644765|    1344.5780| NA          |
| Togo                     | Africa    |  1987|  56.94100|     3154264|    1202.2014| NA          |
| Togo                     | Africa    |  1992|  58.06100|     3747553|    1034.2989| NA          |
| Togo                     | Africa    |  1997|  58.39000|     4320890|     982.2869| NA          |
| Togo                     | Africa    |  2002|  57.56100|     4977378|     886.2206| NA          |
| Togo                     | Africa    |  2007|  58.42000|     5701579|     882.9699| NA          |
| Trinidad and Tobago      | Americas  |  1952|  59.10000|      662850|    3023.2719| NA          |
| Trinidad and Tobago      | Americas  |  1957|  61.80000|      764900|    4100.3934| NA          |
| Trinidad and Tobago      | Americas  |  1962|  64.90000|      887498|    4997.5240| NA          |
| Trinidad and Tobago      | Americas  |  1967|  65.40000|      960155|    5621.3685| NA          |
| Trinidad and Tobago      | Americas  |  1972|  65.90000|      975199|    6619.5514| NA          |
| Trinidad and Tobago      | Americas  |  1977|  68.30000|     1039009|    7899.5542| NA          |
| Trinidad and Tobago      | Americas  |  1982|  68.83200|     1116479|    9119.5286| NA          |
| Trinidad and Tobago      | Americas  |  1987|  69.58200|     1191336|    7388.5978| NA          |
| Trinidad and Tobago      | Americas  |  1992|  69.86200|     1183669|    7370.9909| NA          |
| Trinidad and Tobago      | Americas  |  1997|  69.46500|     1138101|    8792.5731| NA          |
| Trinidad and Tobago      | Americas  |  2002|  68.97600|     1101832|   11460.6002| NA          |
| Trinidad and Tobago      | Americas  |  2007|  69.81900|     1056608|   18008.5092| NA          |
| Tunisia                  | Africa    |  1952|  44.60000|     3647735|    1468.4756| NA          |
| Tunisia                  | Africa    |  1957|  47.10000|     3950849|    1395.2325| NA          |
| Tunisia                  | Africa    |  1962|  49.57900|     4286552|    1660.3032| NA          |
| Tunisia                  | Africa    |  1967|  52.05300|     4786986|    1932.3602| NA          |
| Tunisia                  | Africa    |  1972|  55.60200|     5303507|    2753.2860| NA          |
| Tunisia                  | Africa    |  1977|  59.83700|     6005061|    3120.8768| NA          |
| Tunisia                  | Africa    |  1982|  64.04800|     6734098|    3560.2332| NA          |
| Tunisia                  | Africa    |  1987|  66.89400|     7724976|    3810.4193| NA          |
| Tunisia                  | Africa    |  1992|  70.00100|     8523077|    4332.7202| NA          |
| Tunisia                  | Africa    |  1997|  71.97300|     9231669|    4876.7986| NA          |
| Tunisia                  | Africa    |  2002|  73.04200|     9770575|    5722.8957| NA          |
| Tunisia                  | Africa    |  2007|  73.92300|    10276158|    7092.9230| NA          |
| Turkey                   | Europe    |  1952|  43.58500|    22235677|    1969.1010| Ankara      |
| Turkey                   | Europe    |  1957|  48.07900|    25670939|    2218.7543| Ankara      |
| Turkey                   | Europe    |  1962|  52.09800|    29788695|    2322.8699| Ankara      |
| Turkey                   | Europe    |  1967|  54.33600|    33411317|    2826.3564| Ankara      |
| Turkey                   | Europe    |  1972|  57.00500|    37492953|    3450.6964| Ankara      |
| Turkey                   | Europe    |  1977|  59.50700|    42404033|    4269.1223| Ankara      |
| Turkey                   | Europe    |  1982|  61.03600|    47328791|    4241.3563| Ankara      |
| Turkey                   | Europe    |  1987|  63.10800|    52881328|    5089.0437| Ankara      |
| Turkey                   | Europe    |  1992|  66.14600|    58179144|    5678.3483| Ankara      |
| Turkey                   | Europe    |  1997|  68.83500|    63047647|    6601.4299| Ankara      |
| Turkey                   | Europe    |  2002|  70.84500|    67308928|    6508.0857| Ankara      |
| Turkey                   | Europe    |  2007|  71.77700|    71158647|    8458.2764| Ankara      |
| Uganda                   | Africa    |  1952|  39.97800|     5824797|     734.7535| NA          |
| Uganda                   | Africa    |  1957|  42.57100|     6675501|     774.3711| NA          |
| Uganda                   | Africa    |  1962|  45.34400|     7688797|     767.2717| NA          |
| Uganda                   | Africa    |  1967|  48.05100|     8900294|     908.9185| NA          |
| Uganda                   | Africa    |  1972|  51.01600|    10190285|     950.7359| NA          |
| Uganda                   | Africa    |  1977|  50.35000|    11457758|     843.7331| NA          |
| Uganda                   | Africa    |  1982|  49.84900|    12939400|     682.2662| NA          |
| Uganda                   | Africa    |  1987|  51.50900|    15283050|     617.7244| NA          |
| Uganda                   | Africa    |  1992|  48.82500|    18252190|     644.1708| NA          |
| Uganda                   | Africa    |  1997|  44.57800|    21210254|     816.5591| NA          |
| Uganda                   | Africa    |  2002|  47.81300|    24739869|     927.7210| NA          |
| Uganda                   | Africa    |  2007|  51.54200|    29170398|    1056.3801| NA          |
| United Kingdom           | Europe    |  1952|  69.18000|    50430000|    9979.5085| NA          |
| United Kingdom           | Europe    |  1957|  70.42000|    51430000|   11283.1779| NA          |
| United Kingdom           | Europe    |  1962|  70.76000|    53292000|   12477.1771| NA          |
| United Kingdom           | Europe    |  1967|  71.36000|    54959000|   14142.8509| NA          |
| United Kingdom           | Europe    |  1972|  72.01000|    56079000|   15895.1164| NA          |
| United Kingdom           | Europe    |  1977|  72.76000|    56179000|   17428.7485| NA          |
| United Kingdom           | Europe    |  1982|  74.04000|    56339704|   18232.4245| NA          |
| United Kingdom           | Europe    |  1987|  75.00700|    56981620|   21664.7877| NA          |
| United Kingdom           | Europe    |  1992|  76.42000|    57866349|   22705.0925| NA          |
| United Kingdom           | Europe    |  1997|  77.21800|    58808266|   26074.5314| NA          |
| United Kingdom           | Europe    |  2002|  78.47100|    59912431|   29478.9992| NA          |
| United Kingdom           | Europe    |  2007|  79.42500|    60776238|   33203.2613| NA          |
| United States            | Americas  |  1952|  68.44000|   157553000|   13990.4821| Washington  |
| United States            | Americas  |  1957|  69.49000|   171984000|   14847.1271| Washington  |
| United States            | Americas  |  1962|  70.21000|   186538000|   16173.1459| Washington  |
| United States            | Americas  |  1967|  70.76000|   198712000|   19530.3656| Washington  |
| United States            | Americas  |  1972|  71.34000|   209896000|   21806.0359| Washington  |
| United States            | Americas  |  1977|  73.38000|   220239000|   24072.6321| Washington  |
| United States            | Americas  |  1982|  74.65000|   232187835|   25009.5591| Washington  |
| United States            | Americas  |  1987|  75.02000|   242803533|   29884.3504| Washington  |
| United States            | Americas  |  1992|  76.09000|   256894189|   32003.9322| Washington  |
| United States            | Americas  |  1997|  76.81000|   272911760|   35767.4330| Washington  |
| United States            | Americas  |  2002|  77.31000|   287675526|   39097.0995| Washington  |
| United States            | Americas  |  2007|  78.24200|   301139947|   42951.6531| Washington  |
| Uruguay                  | Americas  |  1952|  66.07100|     2252965|    5716.7667| Montevideo  |
| Uruguay                  | Americas  |  1957|  67.04400|     2424959|    6150.7730| Montevideo  |
| Uruguay                  | Americas  |  1962|  68.25300|     2598466|    5603.3577| Montevideo  |
| Uruguay                  | Americas  |  1967|  68.46800|     2748579|    5444.6196| Montevideo  |
| Uruguay                  | Americas  |  1972|  68.67300|     2829526|    5703.4089| Montevideo  |
| Uruguay                  | Americas  |  1977|  69.48100|     2873520|    6504.3397| Montevideo  |
| Uruguay                  | Americas  |  1982|  70.80500|     2953997|    6920.2231| Montevideo  |
| Uruguay                  | Americas  |  1987|  71.91800|     3045153|    7452.3990| Montevideo  |
| Uruguay                  | Americas  |  1992|  72.75200|     3149262|    8137.0048| Montevideo  |
| Uruguay                  | Americas  |  1997|  74.22300|     3262838|    9230.2407| Montevideo  |
| Uruguay                  | Americas  |  2002|  75.30700|     3363085|    7727.0020| Montevideo  |
| Uruguay                  | Americas  |  2007|  76.38400|     3447496|   10611.4630| Montevideo  |
| Venezuela                | Americas  |  1952|  55.08800|     5439568|    7689.7998| NA          |
| Venezuela                | Americas  |  1957|  57.90700|     6702668|    9802.4665| NA          |
| Venezuela                | Americas  |  1962|  60.77000|     8143375|    8422.9742| NA          |
| Venezuela                | Americas  |  1967|  63.47900|     9709552|    9541.4742| NA          |
| Venezuela                | Americas  |  1972|  65.71200|    11515649|   10505.2597| NA          |
| Venezuela                | Americas  |  1977|  67.45600|    13503563|   13143.9510| NA          |
| Venezuela                | Americas  |  1982|  68.55700|    15620766|   11152.4101| NA          |
| Venezuela                | Americas  |  1987|  70.19000|    17910182|    9883.5846| NA          |
| Venezuela                | Americas  |  1992|  71.15000|    20265563|   10733.9263| NA          |
| Venezuela                | Americas  |  1997|  72.14600|    22374398|   10165.4952| NA          |
| Venezuela                | Americas  |  2002|  72.76600|    24287670|    8605.0478| NA          |
| Venezuela                | Americas  |  2007|  73.74700|    26084662|   11415.8057| NA          |
| Vietnam                  | Asia      |  1952|  40.41200|    26246839|     605.0665| NA          |
| Vietnam                  | Asia      |  1957|  42.88700|    28998543|     676.2854| NA          |
| Vietnam                  | Asia      |  1962|  45.36300|    33796140|     772.0492| NA          |
| Vietnam                  | Asia      |  1967|  47.83800|    39463910|     637.1233| NA          |
| Vietnam                  | Asia      |  1972|  50.25400|    44655014|     699.5016| NA          |
| Vietnam                  | Asia      |  1977|  55.76400|    50533506|     713.5371| NA          |
| Vietnam                  | Asia      |  1982|  58.81600|    56142181|     707.2358| NA          |
| Vietnam                  | Asia      |  1987|  62.82000|    62826491|     820.7994| NA          |
| Vietnam                  | Asia      |  1992|  67.66200|    69940728|     989.0231| NA          |
| Vietnam                  | Asia      |  1997|  70.67200|    76048996|    1385.8968| NA          |
| Vietnam                  | Asia      |  2002|  73.01700|    80908147|    1764.4567| NA          |
| Vietnam                  | Asia      |  2007|  74.24900|    85262356|    2441.5764| NA          |
| West Bank and Gaza       | Asia      |  1952|  43.16000|     1030585|    1515.5923| NA          |
| West Bank and Gaza       | Asia      |  1957|  45.67100|     1070439|    1827.0677| NA          |
| West Bank and Gaza       | Asia      |  1962|  48.12700|     1133134|    2198.9563| NA          |
| West Bank and Gaza       | Asia      |  1967|  51.63100|     1142636|    2649.7150| NA          |
| West Bank and Gaza       | Asia      |  1972|  56.53200|     1089572|    3133.4093| NA          |
| West Bank and Gaza       | Asia      |  1977|  60.76500|     1261091|    3682.8315| NA          |
| West Bank and Gaza       | Asia      |  1982|  64.40600|     1425876|    4336.0321| NA          |
| West Bank and Gaza       | Asia      |  1987|  67.04600|     1691210|    5107.1974| NA          |
| West Bank and Gaza       | Asia      |  1992|  69.71800|     2104779|    6017.6548| NA          |
| West Bank and Gaza       | Asia      |  1997|  71.09600|     2826046|    7110.6676| NA          |
| West Bank and Gaza       | Asia      |  2002|  72.37000|     3389578|    4515.4876| NA          |
| West Bank and Gaza       | Asia      |  2007|  73.42200|     4018332|    3025.3498| NA          |
| Yemen, Rep.              | Asia      |  1952|  32.54800|     4963829|     781.7176| NA          |
| Yemen, Rep.              | Asia      |  1957|  33.97000|     5498090|     804.8305| NA          |
| Yemen, Rep.              | Asia      |  1962|  35.18000|     6120081|     825.6232| NA          |
| Yemen, Rep.              | Asia      |  1967|  36.98400|     6740785|     862.4421| NA          |
| Yemen, Rep.              | Asia      |  1972|  39.84800|     7407075|    1265.0470| NA          |
| Yemen, Rep.              | Asia      |  1977|  44.17500|     8403990|    1829.7652| NA          |
| Yemen, Rep.              | Asia      |  1982|  49.11300|     9657618|    1977.5570| NA          |
| Yemen, Rep.              | Asia      |  1987|  52.92200|    11219340|    1971.7415| NA          |
| Yemen, Rep.              | Asia      |  1992|  55.59900|    13367997|    1879.4967| NA          |
| Yemen, Rep.              | Asia      |  1997|  58.02000|    15826497|    2117.4845| NA          |
| Yemen, Rep.              | Asia      |  2002|  60.30800|    18701257|    2234.8208| NA          |
| Yemen, Rep.              | Asia      |  2007|  62.69800|    22211743|    2280.7699| NA          |
| Zambia                   | Africa    |  1952|  42.03800|     2672000|    1147.3888| NA          |
| Zambia                   | Africa    |  1957|  44.07700|     3016000|    1311.9568| NA          |
| Zambia                   | Africa    |  1962|  46.02300|     3421000|    1452.7258| NA          |
| Zambia                   | Africa    |  1967|  47.76800|     3900000|    1777.0773| NA          |
| Zambia                   | Africa    |  1972|  50.10700|     4506497|    1773.4983| NA          |
| Zambia                   | Africa    |  1977|  51.38600|     5216550|    1588.6883| NA          |
| Zambia                   | Africa    |  1982|  51.82100|     6100407|    1408.6786| NA          |
| Zambia                   | Africa    |  1987|  50.82100|     7272406|    1213.3151| NA          |
| Zambia                   | Africa    |  1992|  46.10000|     8381163|    1210.8846| NA          |
| Zambia                   | Africa    |  1997|  40.23800|     9417789|    1071.3538| NA          |
| Zambia                   | Africa    |  2002|  39.19300|    10595811|    1071.6139| NA          |
| Zambia                   | Africa    |  2007|  42.38400|    11746035|    1271.2116| NA          |
| Zimbabwe                 | Africa    |  1952|  48.45100|     3080907|     406.8841| Harare      |
| Zimbabwe                 | Africa    |  1957|  50.46900|     3646340|     518.7643| Harare      |
| Zimbabwe                 | Africa    |  1962|  52.35800|     4277736|     527.2722| Harare      |
| Zimbabwe                 | Africa    |  1967|  53.99500|     4995432|     569.7951| Harare      |
| Zimbabwe                 | Africa    |  1972|  55.63500|     5861135|     799.3622| Harare      |
| Zimbabwe                 | Africa    |  1977|  57.67400|     6642107|     685.5877| Harare      |
| Zimbabwe                 | Africa    |  1982|  60.36300|     7636524|     788.8550| Harare      |
| Zimbabwe                 | Africa    |  1987|  62.35100|     9216418|     706.1573| Harare      |
| Zimbabwe                 | Africa    |  1992|  60.37700|    10704340|     693.4208| Harare      |
| Zimbabwe                 | Africa    |  1997|  46.80900|    11404948|     792.4500| Harare      |
| Zimbabwe                 | Africa    |  2002|  39.98900|    11926563|     672.0386| Harare      |
| Zimbabwe                 | Africa    |  2007|  43.48700|    12311143|     469.7093| Harare      |

The left join merges both tables and keeps all of the rows in the "left" dataset ie gapminder. The merge happens using the column with the same name in both data frames ie country. The resulting table consists of all 1704 rows from gapminder and 7 columns (one additional from countries\_18). If there is no information in countries\_18 table, then there will be NA added in the `capital` column. The country that is not in gapminder but is in countries\_18 is removed.

``` r
joined_right <- right_join(gapminder, countries_18)
```

    ## Joining, by = "country"

    ## Warning: Column `country` joining factor and character vector, coercing
    ## into character vector

``` r
str(joined_right)
```

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    205 obs. of  7 variables:
    ##  $ country  : chr  "Canada" "Canada" "Canada" "Canada" ...
    ##  $ continent: Factor w/ 5 levels "Africa","Americas",..: 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ year     : int  1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
    ##  $ lifeExp  : num  68.8 70 71.3 72.1 72.9 ...
    ##  $ pop      : int  14785584 17010154 18985849 20819767 22284500 23796400 25201900 26549700 28523502 30305843 ...
    ##  $ gdpPercap: num  11367 12490 13462 16077 18971 ...
    ##  $ capital  : chr  "Ottawa" "Ottawa" "Ottawa" "Ottawa" ...

``` r
levels(as.factor(joined_right$country))
```

    ##  [1] "Australia"     "Brazil"        "Canada"        "China"        
    ##  [5] "Ethiopia"      "France"        "Greece"        "Japan"        
    ##  [9] "Mexico"        "New Zealand"   "Norway"        "Russia"       
    ## [13] "Somalia"       "Thailand"      "Turkey"        "United States"
    ## [17] "Uruguay"       "Zimbabwe"

``` r
levels(as.factor(joined_right$year))
```

    ##  [1] "1952" "1957" "1962" "1967" "1972" "1977" "1982" "1987" "1992" "1997"
    ## [11] "2002" "2007"

``` r
knitr::kable(joined_right)
```

| country       | continent |  year|   lifeExp|         pop|   gdpPercap| capital     |
|:--------------|:----------|-----:|---------:|-----------:|-----------:|:------------|
| Canada        | Americas  |  1952|  68.75000|    14785584|  11367.1611| Ottawa      |
| Canada        | Americas  |  1957|  69.96000|    17010154|  12489.9501| Ottawa      |
| Canada        | Americas  |  1962|  71.30000|    18985849|  13462.4855| Ottawa      |
| Canada        | Americas  |  1967|  72.13000|    20819767|  16076.5880| Ottawa      |
| Canada        | Americas  |  1972|  72.88000|    22284500|  18970.5709| Ottawa      |
| Canada        | Americas  |  1977|  74.21000|    23796400|  22090.8831| Ottawa      |
| Canada        | Americas  |  1982|  75.76000|    25201900|  22898.7921| Ottawa      |
| Canada        | Americas  |  1987|  76.86000|    26549700|  26626.5150| Ottawa      |
| Canada        | Americas  |  1992|  77.95000|    28523502|  26342.8843| Ottawa      |
| Canada        | Americas  |  1997|  78.61000|    30305843|  28954.9259| Ottawa      |
| Canada        | Americas  |  2002|  79.77000|    31902268|  33328.9651| Ottawa      |
| Canada        | Americas  |  2007|  80.65300|    33390141|  36319.2350| Ottawa      |
| Australia     | Oceania   |  1952|  69.12000|     8691212|  10039.5956| Canberra    |
| Australia     | Oceania   |  1957|  70.33000|     9712569|  10949.6496| Canberra    |
| Australia     | Oceania   |  1962|  70.93000|    10794968|  12217.2269| Canberra    |
| Australia     | Oceania   |  1967|  71.10000|    11872264|  14526.1246| Canberra    |
| Australia     | Oceania   |  1972|  71.93000|    13177000|  16788.6295| Canberra    |
| Australia     | Oceania   |  1977|  73.49000|    14074100|  18334.1975| Canberra    |
| Australia     | Oceania   |  1982|  74.74000|    15184200|  19477.0093| Canberra    |
| Australia     | Oceania   |  1987|  76.32000|    16257249|  21888.8890| Canberra    |
| Australia     | Oceania   |  1992|  77.56000|    17481977|  23424.7668| Canberra    |
| Australia     | Oceania   |  1997|  78.83000|    18565243|  26997.9366| Canberra    |
| Australia     | Oceania   |  2002|  80.37000|    19546792|  30687.7547| Canberra    |
| Australia     | Oceania   |  2007|  81.23500|    20434176|  34435.3674| Canberra    |
| New Zealand   | Oceania   |  1952|  69.39000|     1994794|  10556.5757| Wellington  |
| New Zealand   | Oceania   |  1957|  70.26000|     2229407|  12247.3953| Wellington  |
| New Zealand   | Oceania   |  1962|  71.24000|     2488550|  13175.6780| Wellington  |
| New Zealand   | Oceania   |  1967|  71.52000|     2728150|  14463.9189| Wellington  |
| New Zealand   | Oceania   |  1972|  71.89000|     2929100|  16046.0373| Wellington  |
| New Zealand   | Oceania   |  1977|  72.22000|     3164900|  16233.7177| Wellington  |
| New Zealand   | Oceania   |  1982|  73.84000|     3210650|  17632.4104| Wellington  |
| New Zealand   | Oceania   |  1987|  74.32000|     3317166|  19007.1913| Wellington  |
| New Zealand   | Oceania   |  1992|  76.33000|     3437674|  18363.3249| Wellington  |
| New Zealand   | Oceania   |  1997|  77.55000|     3676187|  21050.4138| Wellington  |
| New Zealand   | Oceania   |  2002|  79.11000|     3908037|  23189.8014| Wellington  |
| New Zealand   | Oceania   |  2007|  80.20400|     4115771|  25185.0091| Wellington  |
| United States | Americas  |  1952|  68.44000|   157553000|  13990.4821| Washington  |
| United States | Americas  |  1957|  69.49000|   171984000|  14847.1271| Washington  |
| United States | Americas  |  1962|  70.21000|   186538000|  16173.1459| Washington  |
| United States | Americas  |  1967|  70.76000|   198712000|  19530.3656| Washington  |
| United States | Americas  |  1972|  71.34000|   209896000|  21806.0359| Washington  |
| United States | Americas  |  1977|  73.38000|   220239000|  24072.6321| Washington  |
| United States | Americas  |  1982|  74.65000|   232187835|  25009.5591| Washington  |
| United States | Americas  |  1987|  75.02000|   242803533|  29884.3504| Washington  |
| United States | Americas  |  1992|  76.09000|   256894189|  32003.9322| Washington  |
| United States | Americas  |  1997|  76.81000|   272911760|  35767.4330| Washington  |
| United States | Americas  |  2002|  77.31000|   287675526|  39097.0995| Washington  |
| United States | Americas  |  2007|  78.24200|   301139947|  42951.6531| Washington  |
| Mexico        | Americas  |  1952|  50.78900|    30144317|   3478.1255| Mexico City |
| Mexico        | Americas  |  1957|  55.19000|    35015548|   4131.5466| Mexico City |
| Mexico        | Americas  |  1962|  58.29900|    41121485|   4581.6094| Mexico City |
| Mexico        | Americas  |  1967|  60.11000|    47995559|   5754.7339| Mexico City |
| Mexico        | Americas  |  1972|  62.36100|    55984294|   6809.4067| Mexico City |
| Mexico        | Americas  |  1977|  65.03200|    63759976|   7674.9291| Mexico City |
| Mexico        | Americas  |  1982|  67.40500|    71640904|   9611.1475| Mexico City |
| Mexico        | Americas  |  1987|  69.49800|    80122492|   8688.1560| Mexico City |
| Mexico        | Americas  |  1992|  71.45500|    88111030|   9472.3843| Mexico City |
| Mexico        | Americas  |  1997|  73.67000|    95895146|   9767.2975| Mexico City |
| Mexico        | Americas  |  2002|  74.90200|   102479927|  10742.4405| Mexico City |
| Mexico        | Americas  |  2007|  76.19500|   108700891|  11977.5750| Mexico City |
| France        | Europe    |  1952|  67.41000|    42459667|   7029.8093| Paris       |
| France        | Europe    |  1957|  68.93000|    44310863|   8662.8349| Paris       |
| France        | Europe    |  1962|  70.51000|    47124000|  10560.4855| Paris       |
| France        | Europe    |  1967|  71.55000|    49569000|  12999.9177| Paris       |
| France        | Europe    |  1972|  72.38000|    51732000|  16107.1917| Paris       |
| France        | Europe    |  1977|  73.83000|    53165019|  18292.6351| Paris       |
| France        | Europe    |  1982|  74.89000|    54433565|  20293.8975| Paris       |
| France        | Europe    |  1987|  76.34000|    55630100|  22066.4421| Paris       |
| France        | Europe    |  1992|  77.46000|    57374179|  24703.7961| Paris       |
| France        | Europe    |  1997|  78.64000|    58623428|  25889.7849| Paris       |
| France        | Europe    |  2002|  79.59000|    59925035|  28926.0323| Paris       |
| France        | Europe    |  2007|  80.65700|    61083916|  30470.0167| Paris       |
| Norway        | Europe    |  1952|  72.67000|     3327728|  10095.4217| Oslo        |
| Norway        | Europe    |  1957|  73.44000|     3491938|  11653.9730| Oslo        |
| Norway        | Europe    |  1962|  73.47000|     3638919|  13450.4015| Oslo        |
| Norway        | Europe    |  1967|  74.08000|     3786019|  16361.8765| Oslo        |
| Norway        | Europe    |  1972|  74.34000|     3933004|  18965.0555| Oslo        |
| Norway        | Europe    |  1977|  75.37000|     4043205|  23311.3494| Oslo        |
| Norway        | Europe    |  1982|  75.97000|     4114787|  26298.6353| Oslo        |
| Norway        | Europe    |  1987|  75.89000|     4186147|  31540.9748| Oslo        |
| Norway        | Europe    |  1992|  77.32000|     4286357|  33965.6611| Oslo        |
| Norway        | Europe    |  1997|  78.32000|     4405672|  41283.1643| Oslo        |
| Norway        | Europe    |  2002|  79.05000|     4535591|  44683.9753| Oslo        |
| Norway        | Europe    |  2007|  80.19600|     4627926|  49357.1902| Oslo        |
| Greece        | Europe    |  1952|  65.86000|     7733250|   3530.6901| Athens      |
| Greece        | Europe    |  1957|  67.86000|     8096218|   4916.2999| Athens      |
| Greece        | Europe    |  1962|  69.51000|     8448233|   6017.1907| Athens      |
| Greece        | Europe    |  1967|  71.00000|     8716441|   8513.0970| Athens      |
| Greece        | Europe    |  1972|  72.34000|     8888628|  12724.8296| Athens      |
| Greece        | Europe    |  1977|  73.68000|     9308479|  14195.5243| Athens      |
| Greece        | Europe    |  1982|  75.24000|     9786480|  15268.4209| Athens      |
| Greece        | Europe    |  1987|  76.67000|     9974490|  16120.5284| Athens      |
| Greece        | Europe    |  1992|  77.03000|    10325429|  17541.4963| Athens      |
| Greece        | Europe    |  1997|  77.86900|    10502372|  18747.6981| Athens      |
| Greece        | Europe    |  2002|  78.25600|    10603863|  22514.2548| Athens      |
| Greece        | Europe    |  2007|  79.48300|    10706290|  27538.4119| Athens      |
| China         | Asia      |  1952|  44.00000|   556263527|    400.4486| Beijing     |
| China         | Asia      |  1957|  50.54896|   637408000|    575.9870| Beijing     |
| China         | Asia      |  1962|  44.50136|   665770000|    487.6740| Beijing     |
| China         | Asia      |  1967|  58.38112|   754550000|    612.7057| Beijing     |
| China         | Asia      |  1972|  63.11888|   862030000|    676.9001| Beijing     |
| China         | Asia      |  1977|  63.96736|   943455000|    741.2375| Beijing     |
| China         | Asia      |  1982|  65.52500|  1000281000|    962.4214| Beijing     |
| China         | Asia      |  1987|  67.27400|  1084035000|   1378.9040| Beijing     |
| China         | Asia      |  1992|  68.69000|  1164970000|   1655.7842| Beijing     |
| China         | Asia      |  1997|  70.42600|  1230075000|   2289.2341| Beijing     |
| China         | Asia      |  2002|  72.02800|  1280400000|   3119.2809| Beijing     |
| China         | Asia      |  2007|  72.96100|  1318683096|   4959.1149| Beijing     |
| Japan         | Asia      |  1952|  63.03000|    86459025|   3216.9563| Tokyo       |
| Japan         | Asia      |  1957|  65.50000|    91563009|   4317.6944| Tokyo       |
| Japan         | Asia      |  1962|  68.73000|    95831757|   6576.6495| Tokyo       |
| Japan         | Asia      |  1967|  71.43000|   100825279|   9847.7886| Tokyo       |
| Japan         | Asia      |  1972|  73.42000|   107188273|  14778.7864| Tokyo       |
| Japan         | Asia      |  1977|  75.38000|   113872473|  16610.3770| Tokyo       |
| Japan         | Asia      |  1982|  77.11000|   118454974|  19384.1057| Tokyo       |
| Japan         | Asia      |  1987|  78.67000|   122091325|  22375.9419| Tokyo       |
| Japan         | Asia      |  1992|  79.36000|   124329269|  26824.8951| Tokyo       |
| Japan         | Asia      |  1997|  80.69000|   125956499|  28816.5850| Tokyo       |
| Japan         | Asia      |  2002|  82.00000|   127065841|  28604.5919| Tokyo       |
| Japan         | Asia      |  2007|  82.60300|   127467972|  31656.0681| Tokyo       |
| Turkey        | Europe    |  1952|  43.58500|    22235677|   1969.1010| Ankara      |
| Turkey        | Europe    |  1957|  48.07900|    25670939|   2218.7543| Ankara      |
| Turkey        | Europe    |  1962|  52.09800|    29788695|   2322.8699| Ankara      |
| Turkey        | Europe    |  1967|  54.33600|    33411317|   2826.3564| Ankara      |
| Turkey        | Europe    |  1972|  57.00500|    37492953|   3450.6964| Ankara      |
| Turkey        | Europe    |  1977|  59.50700|    42404033|   4269.1223| Ankara      |
| Turkey        | Europe    |  1982|  61.03600|    47328791|   4241.3563| Ankara      |
| Turkey        | Europe    |  1987|  63.10800|    52881328|   5089.0437| Ankara      |
| Turkey        | Europe    |  1992|  66.14600|    58179144|   5678.3483| Ankara      |
| Turkey        | Europe    |  1997|  68.83500|    63047647|   6601.4299| Ankara      |
| Turkey        | Europe    |  2002|  70.84500|    67308928|   6508.0857| Ankara      |
| Turkey        | Europe    |  2007|  71.77700|    71158647|   8458.2764| Ankara      |
| Zimbabwe      | Africa    |  1952|  48.45100|     3080907|    406.8841| Harare      |
| Zimbabwe      | Africa    |  1957|  50.46900|     3646340|    518.7643| Harare      |
| Zimbabwe      | Africa    |  1962|  52.35800|     4277736|    527.2722| Harare      |
| Zimbabwe      | Africa    |  1967|  53.99500|     4995432|    569.7951| Harare      |
| Zimbabwe      | Africa    |  1972|  55.63500|     5861135|    799.3622| Harare      |
| Zimbabwe      | Africa    |  1977|  57.67400|     6642107|    685.5877| Harare      |
| Zimbabwe      | Africa    |  1982|  60.36300|     7636524|    788.8550| Harare      |
| Zimbabwe      | Africa    |  1987|  62.35100|     9216418|    706.1573| Harare      |
| Zimbabwe      | Africa    |  1992|  60.37700|    10704340|    693.4208| Harare      |
| Zimbabwe      | Africa    |  1997|  46.80900|    11404948|    792.4500| Harare      |
| Zimbabwe      | Africa    |  2002|  39.98900|    11926563|    672.0386| Harare      |
| Zimbabwe      | Africa    |  2007|  43.48700|    12311143|    469.7093| Harare      |
| Uruguay       | Americas  |  1952|  66.07100|     2252965|   5716.7667| Montevideo  |
| Uruguay       | Americas  |  1957|  67.04400|     2424959|   6150.7730| Montevideo  |
| Uruguay       | Americas  |  1962|  68.25300|     2598466|   5603.3577| Montevideo  |
| Uruguay       | Americas  |  1967|  68.46800|     2748579|   5444.6196| Montevideo  |
| Uruguay       | Americas  |  1972|  68.67300|     2829526|   5703.4089| Montevideo  |
| Uruguay       | Americas  |  1977|  69.48100|     2873520|   6504.3397| Montevideo  |
| Uruguay       | Americas  |  1982|  70.80500|     2953997|   6920.2231| Montevideo  |
| Uruguay       | Americas  |  1987|  71.91800|     3045153|   7452.3990| Montevideo  |
| Uruguay       | Americas  |  1992|  72.75200|     3149262|   8137.0048| Montevideo  |
| Uruguay       | Americas  |  1997|  74.22300|     3262838|   9230.2407| Montevideo  |
| Uruguay       | Americas  |  2002|  75.30700|     3363085|   7727.0020| Montevideo  |
| Uruguay       | Americas  |  2007|  76.38400|     3447496|  10611.4630| Montevideo  |
| Brazil        | Americas  |  1952|  50.91700|    56602560|   2108.9444| Brasilia    |
| Brazil        | Americas  |  1957|  53.28500|    65551171|   2487.3660| Brasilia    |
| Brazil        | Americas  |  1962|  55.66500|    76039390|   3336.5858| Brasilia    |
| Brazil        | Americas  |  1967|  57.63200|    88049823|   3429.8644| Brasilia    |
| Brazil        | Americas  |  1972|  59.50400|   100840058|   4985.7115| Brasilia    |
| Brazil        | Americas  |  1977|  61.48900|   114313951|   6660.1187| Brasilia    |
| Brazil        | Americas  |  1982|  63.33600|   128962939|   7030.8359| Brasilia    |
| Brazil        | Americas  |  1987|  65.20500|   142938076|   7807.0958| Brasilia    |
| Brazil        | Americas  |  1992|  67.05700|   155975974|   6950.2830| Brasilia    |
| Brazil        | Americas  |  1997|  69.38800|   168546719|   7957.9808| Brasilia    |
| Brazil        | Americas  |  2002|  71.00600|   179914212|   8131.2128| Brasilia    |
| Brazil        | Americas  |  2007|  72.39000|   190010647|   9065.8008| Brasilia    |
| Somalia       | Africa    |  1952|  32.97800|     2526994|   1135.7498| Mogadishu   |
| Somalia       | Africa    |  1957|  34.97700|     2780415|   1258.1474| Mogadishu   |
| Somalia       | Africa    |  1962|  36.98100|     3080153|   1369.4883| Mogadishu   |
| Somalia       | Africa    |  1967|  38.97700|     3428839|   1284.7332| Mogadishu   |
| Somalia       | Africa    |  1972|  40.97300|     3840161|   1254.5761| Mogadishu   |
| Somalia       | Africa    |  1977|  41.97400|     4353666|   1450.9925| Mogadishu   |
| Somalia       | Africa    |  1982|  42.95500|     5828892|   1176.8070| Mogadishu   |
| Somalia       | Africa    |  1987|  44.50100|     6921858|   1093.2450| Mogadishu   |
| Somalia       | Africa    |  1992|  39.65800|     6099799|    926.9603| Mogadishu   |
| Somalia       | Africa    |  1997|  43.79500|     6633514|    930.5964| Mogadishu   |
| Somalia       | Africa    |  2002|  45.93600|     7753310|    882.0818| Mogadishu   |
| Somalia       | Africa    |  2007|  48.15900|     9118773|    926.1411| Mogadishu   |
| Thailand      | Asia      |  1952|  50.84800|    21289402|    757.7974| Bangkok     |
| Thailand      | Asia      |  1957|  53.63000|    25041917|    793.5774| Bangkok     |
| Thailand      | Asia      |  1962|  56.06100|    29263397|   1002.1992| Bangkok     |
| Thailand      | Asia      |  1967|  58.28500|    34024249|   1295.4607| Bangkok     |
| Thailand      | Asia      |  1972|  60.40500|    39276153|   1524.3589| Bangkok     |
| Thailand      | Asia      |  1977|  62.49400|    44148285|   1961.2246| Bangkok     |
| Thailand      | Asia      |  1982|  64.59700|    48827160|   2393.2198| Bangkok     |
| Thailand      | Asia      |  1987|  66.08400|    52910342|   2982.6538| Bangkok     |
| Thailand      | Asia      |  1992|  67.29800|    56667095|   4616.8965| Bangkok     |
| Thailand      | Asia      |  1997|  67.52100|    60216677|   5852.6255| Bangkok     |
| Thailand      | Asia      |  2002|  68.56400|    62806748|   5913.1875| Bangkok     |
| Thailand      | Asia      |  2007|  70.61600|    65068149|   7458.3963| Bangkok     |
| Ethiopia      | Africa    |  1952|  34.07800|    20860941|    362.1463| Addis Ababa |
| Ethiopia      | Africa    |  1957|  36.66700|    22815614|    378.9042| Addis Ababa |
| Ethiopia      | Africa    |  1962|  40.05900|    25145372|    419.4564| Addis Ababa |
| Ethiopia      | Africa    |  1967|  42.11500|    27860297|    516.1186| Addis Ababa |
| Ethiopia      | Africa    |  1972|  43.51500|    30770372|    566.2439| Addis Ababa |
| Ethiopia      | Africa    |  1977|  44.51000|    34617799|    556.8084| Addis Ababa |
| Ethiopia      | Africa    |  1982|  44.91600|    38111756|    577.8607| Addis Ababa |
| Ethiopia      | Africa    |  1987|  46.68400|    42999530|    573.7413| Addis Ababa |
| Ethiopia      | Africa    |  1992|  48.09100|    52088559|    421.3535| Addis Ababa |
| Ethiopia      | Africa    |  1997|  49.40200|    59861301|    515.8894| Addis Ababa |
| Ethiopia      | Africa    |  2002|  50.72500|    67946797|    530.0535| Addis Ababa |
| Ethiopia      | Africa    |  2007|  52.94700|    76511887|    690.8056| Addis Ababa |
| Russia        | NA        |    NA|        NA|          NA|          NA| Moscow      |

The resulting table keeps all the rows from countries\_18 regardless of the fact if they are present in gapminder. If a country doesn't have information from there, the NA's are added - eg Russia is not in gapminder so 6 columns that were added from gapminder have values of NA for it.

All the information from gapminder for countries is kept so the table consists of 205 rows (204 for 17 countries + 1 row for one country not in gapminder) and 7 columns (6 from gapminder + one from countries\_18).

``` r
joined_full <- full_join(gapminder, countries_18)
```

    ## Joining, by = "country"

    ## Warning: Column `country` joining factor and character vector, coercing
    ## into character vector

``` r
str(joined_full)
```

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    1705 obs. of  7 variables:
    ##  $ country  : chr  "Afghanistan" "Afghanistan" "Afghanistan" "Afghanistan" ...
    ##  $ continent: Factor w/ 5 levels "Africa","Americas",..: 3 3 3 3 3 3 3 3 3 3 ...
    ##  $ year     : int  1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
    ##  $ lifeExp  : num  28.8 30.3 32 34 36.1 ...
    ##  $ pop      : int  8425333 9240934 10267083 11537966 13079460 14880372 12881816 13867957 16317921 22227415 ...
    ##  $ gdpPercap: num  779 821 853 836 740 ...
    ##  $ capital  : chr  NA NA NA NA ...

``` r
levels(as.factor(joined_full$country))
```

    ##   [1] "Afghanistan"              "Albania"                 
    ##   [3] "Algeria"                  "Angola"                  
    ##   [5] "Argentina"                "Australia"               
    ##   [7] "Austria"                  "Bahrain"                 
    ##   [9] "Bangladesh"               "Belgium"                 
    ##  [11] "Benin"                    "Bolivia"                 
    ##  [13] "Bosnia and Herzegovina"   "Botswana"                
    ##  [15] "Brazil"                   "Bulgaria"                
    ##  [17] "Burkina Faso"             "Burundi"                 
    ##  [19] "Cambodia"                 "Cameroon"                
    ##  [21] "Canada"                   "Central African Republic"
    ##  [23] "Chad"                     "Chile"                   
    ##  [25] "China"                    "Colombia"                
    ##  [27] "Comoros"                  "Congo, Dem. Rep."        
    ##  [29] "Congo, Rep."              "Costa Rica"              
    ##  [31] "Cote d'Ivoire"            "Croatia"                 
    ##  [33] "Cuba"                     "Czech Republic"          
    ##  [35] "Denmark"                  "Djibouti"                
    ##  [37] "Dominican Republic"       "Ecuador"                 
    ##  [39] "Egypt"                    "El Salvador"             
    ##  [41] "Equatorial Guinea"        "Eritrea"                 
    ##  [43] "Ethiopia"                 "Finland"                 
    ##  [45] "France"                   "Gabon"                   
    ##  [47] "Gambia"                   "Germany"                 
    ##  [49] "Ghana"                    "Greece"                  
    ##  [51] "Guatemala"                "Guinea"                  
    ##  [53] "Guinea-Bissau"            "Haiti"                   
    ##  [55] "Honduras"                 "Hong Kong, China"        
    ##  [57] "Hungary"                  "Iceland"                 
    ##  [59] "India"                    "Indonesia"               
    ##  [61] "Iran"                     "Iraq"                    
    ##  [63] "Ireland"                  "Israel"                  
    ##  [65] "Italy"                    "Jamaica"                 
    ##  [67] "Japan"                    "Jordan"                  
    ##  [69] "Kenya"                    "Korea, Dem. Rep."        
    ##  [71] "Korea, Rep."              "Kuwait"                  
    ##  [73] "Lebanon"                  "Lesotho"                 
    ##  [75] "Liberia"                  "Libya"                   
    ##  [77] "Madagascar"               "Malawi"                  
    ##  [79] "Malaysia"                 "Mali"                    
    ##  [81] "Mauritania"               "Mauritius"               
    ##  [83] "Mexico"                   "Mongolia"                
    ##  [85] "Montenegro"               "Morocco"                 
    ##  [87] "Mozambique"               "Myanmar"                 
    ##  [89] "Namibia"                  "Nepal"                   
    ##  [91] "Netherlands"              "New Zealand"             
    ##  [93] "Nicaragua"                "Niger"                   
    ##  [95] "Nigeria"                  "Norway"                  
    ##  [97] "Oman"                     "Pakistan"                
    ##  [99] "Panama"                   "Paraguay"                
    ## [101] "Peru"                     "Philippines"             
    ## [103] "Poland"                   "Portugal"                
    ## [105] "Puerto Rico"              "Reunion"                 
    ## [107] "Romania"                  "Russia"                  
    ## [109] "Rwanda"                   "Sao Tome and Principe"   
    ## [111] "Saudi Arabia"             "Senegal"                 
    ## [113] "Serbia"                   "Sierra Leone"            
    ## [115] "Singapore"                "Slovak Republic"         
    ## [117] "Slovenia"                 "Somalia"                 
    ## [119] "South Africa"             "Spain"                   
    ## [121] "Sri Lanka"                "Sudan"                   
    ## [123] "Swaziland"                "Sweden"                  
    ## [125] "Switzerland"              "Syria"                   
    ## [127] "Taiwan"                   "Tanzania"                
    ## [129] "Thailand"                 "Togo"                    
    ## [131] "Trinidad and Tobago"      "Tunisia"                 
    ## [133] "Turkey"                   "Uganda"                  
    ## [135] "United Kingdom"           "United States"           
    ## [137] "Uruguay"                  "Venezuela"               
    ## [139] "Vietnam"                  "West Bank and Gaza"      
    ## [141] "Yemen, Rep."              "Zambia"                  
    ## [143] "Zimbabwe"

``` r
levels(as.factor(joined_full$year))
```

    ##  [1] "1952" "1957" "1962" "1967" "1972" "1977" "1982" "1987" "1992" "1997"
    ## [11] "2002" "2007"

``` r
knitr::kable(joined_full)
```

| country                  | continent |  year|   lifeExp|         pop|    gdpPercap| capital     |
|:-------------------------|:----------|-----:|---------:|-----------:|------------:|:------------|
| Afghanistan              | Asia      |  1952|  28.80100|     8425333|     779.4453| NA          |
| Afghanistan              | Asia      |  1957|  30.33200|     9240934|     820.8530| NA          |
| Afghanistan              | Asia      |  1962|  31.99700|    10267083|     853.1007| NA          |
| Afghanistan              | Asia      |  1967|  34.02000|    11537966|     836.1971| NA          |
| Afghanistan              | Asia      |  1972|  36.08800|    13079460|     739.9811| NA          |
| Afghanistan              | Asia      |  1977|  38.43800|    14880372|     786.1134| NA          |
| Afghanistan              | Asia      |  1982|  39.85400|    12881816|     978.0114| NA          |
| Afghanistan              | Asia      |  1987|  40.82200|    13867957|     852.3959| NA          |
| Afghanistan              | Asia      |  1992|  41.67400|    16317921|     649.3414| NA          |
| Afghanistan              | Asia      |  1997|  41.76300|    22227415|     635.3414| NA          |
| Afghanistan              | Asia      |  2002|  42.12900|    25268405|     726.7341| NA          |
| Afghanistan              | Asia      |  2007|  43.82800|    31889923|     974.5803| NA          |
| Albania                  | Europe    |  1952|  55.23000|     1282697|    1601.0561| NA          |
| Albania                  | Europe    |  1957|  59.28000|     1476505|    1942.2842| NA          |
| Albania                  | Europe    |  1962|  64.82000|     1728137|    2312.8890| NA          |
| Albania                  | Europe    |  1967|  66.22000|     1984060|    2760.1969| NA          |
| Albania                  | Europe    |  1972|  67.69000|     2263554|    3313.4222| NA          |
| Albania                  | Europe    |  1977|  68.93000|     2509048|    3533.0039| NA          |
| Albania                  | Europe    |  1982|  70.42000|     2780097|    3630.8807| NA          |
| Albania                  | Europe    |  1987|  72.00000|     3075321|    3738.9327| NA          |
| Albania                  | Europe    |  1992|  71.58100|     3326498|    2497.4379| NA          |
| Albania                  | Europe    |  1997|  72.95000|     3428038|    3193.0546| NA          |
| Albania                  | Europe    |  2002|  75.65100|     3508512|    4604.2117| NA          |
| Albania                  | Europe    |  2007|  76.42300|     3600523|    5937.0295| NA          |
| Algeria                  | Africa    |  1952|  43.07700|     9279525|    2449.0082| NA          |
| Algeria                  | Africa    |  1957|  45.68500|    10270856|    3013.9760| NA          |
| Algeria                  | Africa    |  1962|  48.30300|    11000948|    2550.8169| NA          |
| Algeria                  | Africa    |  1967|  51.40700|    12760499|    3246.9918| NA          |
| Algeria                  | Africa    |  1972|  54.51800|    14760787|    4182.6638| NA          |
| Algeria                  | Africa    |  1977|  58.01400|    17152804|    4910.4168| NA          |
| Algeria                  | Africa    |  1982|  61.36800|    20033753|    5745.1602| NA          |
| Algeria                  | Africa    |  1987|  65.79900|    23254956|    5681.3585| NA          |
| Algeria                  | Africa    |  1992|  67.74400|    26298373|    5023.2166| NA          |
| Algeria                  | Africa    |  1997|  69.15200|    29072015|    4797.2951| NA          |
| Algeria                  | Africa    |  2002|  70.99400|    31287142|    5288.0404| NA          |
| Algeria                  | Africa    |  2007|  72.30100|    33333216|    6223.3675| NA          |
| Angola                   | Africa    |  1952|  30.01500|     4232095|    3520.6103| NA          |
| Angola                   | Africa    |  1957|  31.99900|     4561361|    3827.9405| NA          |
| Angola                   | Africa    |  1962|  34.00000|     4826015|    4269.2767| NA          |
| Angola                   | Africa    |  1967|  35.98500|     5247469|    5522.7764| NA          |
| Angola                   | Africa    |  1972|  37.92800|     5894858|    5473.2880| NA          |
| Angola                   | Africa    |  1977|  39.48300|     6162675|    3008.6474| NA          |
| Angola                   | Africa    |  1982|  39.94200|     7016384|    2756.9537| NA          |
| Angola                   | Africa    |  1987|  39.90600|     7874230|    2430.2083| NA          |
| Angola                   | Africa    |  1992|  40.64700|     8735988|    2627.8457| NA          |
| Angola                   | Africa    |  1997|  40.96300|     9875024|    2277.1409| NA          |
| Angola                   | Africa    |  2002|  41.00300|    10866106|    2773.2873| NA          |
| Angola                   | Africa    |  2007|  42.73100|    12420476|    4797.2313| NA          |
| Argentina                | Americas  |  1952|  62.48500|    17876956|    5911.3151| NA          |
| Argentina                | Americas  |  1957|  64.39900|    19610538|    6856.8562| NA          |
| Argentina                | Americas  |  1962|  65.14200|    21283783|    7133.1660| NA          |
| Argentina                | Americas  |  1967|  65.63400|    22934225|    8052.9530| NA          |
| Argentina                | Americas  |  1972|  67.06500|    24779799|    9443.0385| NA          |
| Argentina                | Americas  |  1977|  68.48100|    26983828|   10079.0267| NA          |
| Argentina                | Americas  |  1982|  69.94200|    29341374|    8997.8974| NA          |
| Argentina                | Americas  |  1987|  70.77400|    31620918|    9139.6714| NA          |
| Argentina                | Americas  |  1992|  71.86800|    33958947|    9308.4187| NA          |
| Argentina                | Americas  |  1997|  73.27500|    36203463|   10967.2820| NA          |
| Argentina                | Americas  |  2002|  74.34000|    38331121|    8797.6407| NA          |
| Argentina                | Americas  |  2007|  75.32000|    40301927|   12779.3796| NA          |
| Australia                | Oceania   |  1952|  69.12000|     8691212|   10039.5956| Canberra    |
| Australia                | Oceania   |  1957|  70.33000|     9712569|   10949.6496| Canberra    |
| Australia                | Oceania   |  1962|  70.93000|    10794968|   12217.2269| Canberra    |
| Australia                | Oceania   |  1967|  71.10000|    11872264|   14526.1246| Canberra    |
| Australia                | Oceania   |  1972|  71.93000|    13177000|   16788.6295| Canberra    |
| Australia                | Oceania   |  1977|  73.49000|    14074100|   18334.1975| Canberra    |
| Australia                | Oceania   |  1982|  74.74000|    15184200|   19477.0093| Canberra    |
| Australia                | Oceania   |  1987|  76.32000|    16257249|   21888.8890| Canberra    |
| Australia                | Oceania   |  1992|  77.56000|    17481977|   23424.7668| Canberra    |
| Australia                | Oceania   |  1997|  78.83000|    18565243|   26997.9366| Canberra    |
| Australia                | Oceania   |  2002|  80.37000|    19546792|   30687.7547| Canberra    |
| Australia                | Oceania   |  2007|  81.23500|    20434176|   34435.3674| Canberra    |
| Austria                  | Europe    |  1952|  66.80000|     6927772|    6137.0765| NA          |
| Austria                  | Europe    |  1957|  67.48000|     6965860|    8842.5980| NA          |
| Austria                  | Europe    |  1962|  69.54000|     7129864|   10750.7211| NA          |
| Austria                  | Europe    |  1967|  70.14000|     7376998|   12834.6024| NA          |
| Austria                  | Europe    |  1972|  70.63000|     7544201|   16661.6256| NA          |
| Austria                  | Europe    |  1977|  72.17000|     7568430|   19749.4223| NA          |
| Austria                  | Europe    |  1982|  73.18000|     7574613|   21597.0836| NA          |
| Austria                  | Europe    |  1987|  74.94000|     7578903|   23687.8261| NA          |
| Austria                  | Europe    |  1992|  76.04000|     7914969|   27042.0187| NA          |
| Austria                  | Europe    |  1997|  77.51000|     8069876|   29095.9207| NA          |
| Austria                  | Europe    |  2002|  78.98000|     8148312|   32417.6077| NA          |
| Austria                  | Europe    |  2007|  79.82900|     8199783|   36126.4927| NA          |
| Bahrain                  | Asia      |  1952|  50.93900|      120447|    9867.0848| NA          |
| Bahrain                  | Asia      |  1957|  53.83200|      138655|   11635.7995| NA          |
| Bahrain                  | Asia      |  1962|  56.92300|      171863|   12753.2751| NA          |
| Bahrain                  | Asia      |  1967|  59.92300|      202182|   14804.6727| NA          |
| Bahrain                  | Asia      |  1972|  63.30000|      230800|   18268.6584| NA          |
| Bahrain                  | Asia      |  1977|  65.59300|      297410|   19340.1020| NA          |
| Bahrain                  | Asia      |  1982|  69.05200|      377967|   19211.1473| NA          |
| Bahrain                  | Asia      |  1987|  70.75000|      454612|   18524.0241| NA          |
| Bahrain                  | Asia      |  1992|  72.60100|      529491|   19035.5792| NA          |
| Bahrain                  | Asia      |  1997|  73.92500|      598561|   20292.0168| NA          |
| Bahrain                  | Asia      |  2002|  74.79500|      656397|   23403.5593| NA          |
| Bahrain                  | Asia      |  2007|  75.63500|      708573|   29796.0483| NA          |
| Bangladesh               | Asia      |  1952|  37.48400|    46886859|     684.2442| NA          |
| Bangladesh               | Asia      |  1957|  39.34800|    51365468|     661.6375| NA          |
| Bangladesh               | Asia      |  1962|  41.21600|    56839289|     686.3416| NA          |
| Bangladesh               | Asia      |  1967|  43.45300|    62821884|     721.1861| NA          |
| Bangladesh               | Asia      |  1972|  45.25200|    70759295|     630.2336| NA          |
| Bangladesh               | Asia      |  1977|  46.92300|    80428306|     659.8772| NA          |
| Bangladesh               | Asia      |  1982|  50.00900|    93074406|     676.9819| NA          |
| Bangladesh               | Asia      |  1987|  52.81900|   103764241|     751.9794| NA          |
| Bangladesh               | Asia      |  1992|  56.01800|   113704579|     837.8102| NA          |
| Bangladesh               | Asia      |  1997|  59.41200|   123315288|     972.7700| NA          |
| Bangladesh               | Asia      |  2002|  62.01300|   135656790|    1136.3904| NA          |
| Bangladesh               | Asia      |  2007|  64.06200|   150448339|    1391.2538| NA          |
| Belgium                  | Europe    |  1952|  68.00000|     8730405|    8343.1051| NA          |
| Belgium                  | Europe    |  1957|  69.24000|     8989111|    9714.9606| NA          |
| Belgium                  | Europe    |  1962|  70.25000|     9218400|   10991.2068| NA          |
| Belgium                  | Europe    |  1967|  70.94000|     9556500|   13149.0412| NA          |
| Belgium                  | Europe    |  1972|  71.44000|     9709100|   16672.1436| NA          |
| Belgium                  | Europe    |  1977|  72.80000|     9821800|   19117.9745| NA          |
| Belgium                  | Europe    |  1982|  73.93000|     9856303|   20979.8459| NA          |
| Belgium                  | Europe    |  1987|  75.35000|     9870200|   22525.5631| NA          |
| Belgium                  | Europe    |  1992|  76.46000|    10045622|   25575.5707| NA          |
| Belgium                  | Europe    |  1997|  77.53000|    10199787|   27561.1966| NA          |
| Belgium                  | Europe    |  2002|  78.32000|    10311970|   30485.8838| NA          |
| Belgium                  | Europe    |  2007|  79.44100|    10392226|   33692.6051| NA          |
| Benin                    | Africa    |  1952|  38.22300|     1738315|    1062.7522| NA          |
| Benin                    | Africa    |  1957|  40.35800|     1925173|     959.6011| NA          |
| Benin                    | Africa    |  1962|  42.61800|     2151895|     949.4991| NA          |
| Benin                    | Africa    |  1967|  44.88500|     2427334|    1035.8314| NA          |
| Benin                    | Africa    |  1972|  47.01400|     2761407|    1085.7969| NA          |
| Benin                    | Africa    |  1977|  49.19000|     3168267|    1029.1613| NA          |
| Benin                    | Africa    |  1982|  50.90400|     3641603|    1277.8976| NA          |
| Benin                    | Africa    |  1987|  52.33700|     4243788|    1225.8560| NA          |
| Benin                    | Africa    |  1992|  53.91900|     4981671|    1191.2077| NA          |
| Benin                    | Africa    |  1997|  54.77700|     6066080|    1232.9753| NA          |
| Benin                    | Africa    |  2002|  54.40600|     7026113|    1372.8779| NA          |
| Benin                    | Africa    |  2007|  56.72800|     8078314|    1441.2849| NA          |
| Bolivia                  | Americas  |  1952|  40.41400|     2883315|    2677.3263| NA          |
| Bolivia                  | Americas  |  1957|  41.89000|     3211738|    2127.6863| NA          |
| Bolivia                  | Americas  |  1962|  43.42800|     3593918|    2180.9725| NA          |
| Bolivia                  | Americas  |  1967|  45.03200|     4040665|    2586.8861| NA          |
| Bolivia                  | Americas  |  1972|  46.71400|     4565872|    2980.3313| NA          |
| Bolivia                  | Americas  |  1977|  50.02300|     5079716|    3548.0978| NA          |
| Bolivia                  | Americas  |  1982|  53.85900|     5642224|    3156.5105| NA          |
| Bolivia                  | Americas  |  1987|  57.25100|     6156369|    2753.6915| NA          |
| Bolivia                  | Americas  |  1992|  59.95700|     6893451|    2961.6997| NA          |
| Bolivia                  | Americas  |  1997|  62.05000|     7693188|    3326.1432| NA          |
| Bolivia                  | Americas  |  2002|  63.88300|     8445134|    3413.2627| NA          |
| Bolivia                  | Americas  |  2007|  65.55400|     9119152|    3822.1371| NA          |
| Bosnia and Herzegovina   | Europe    |  1952|  53.82000|     2791000|     973.5332| NA          |
| Bosnia and Herzegovina   | Europe    |  1957|  58.45000|     3076000|    1353.9892| NA          |
| Bosnia and Herzegovina   | Europe    |  1962|  61.93000|     3349000|    1709.6837| NA          |
| Bosnia and Herzegovina   | Europe    |  1967|  64.79000|     3585000|    2172.3524| NA          |
| Bosnia and Herzegovina   | Europe    |  1972|  67.45000|     3819000|    2860.1698| NA          |
| Bosnia and Herzegovina   | Europe    |  1977|  69.86000|     4086000|    3528.4813| NA          |
| Bosnia and Herzegovina   | Europe    |  1982|  70.69000|     4172693|    4126.6132| NA          |
| Bosnia and Herzegovina   | Europe    |  1987|  71.14000|     4338977|    4314.1148| NA          |
| Bosnia and Herzegovina   | Europe    |  1992|  72.17800|     4256013|    2546.7814| NA          |
| Bosnia and Herzegovina   | Europe    |  1997|  73.24400|     3607000|    4766.3559| NA          |
| Bosnia and Herzegovina   | Europe    |  2002|  74.09000|     4165416|    6018.9752| NA          |
| Bosnia and Herzegovina   | Europe    |  2007|  74.85200|     4552198|    7446.2988| NA          |
| Botswana                 | Africa    |  1952|  47.62200|      442308|     851.2411| NA          |
| Botswana                 | Africa    |  1957|  49.61800|      474639|     918.2325| NA          |
| Botswana                 | Africa    |  1962|  51.52000|      512764|     983.6540| NA          |
| Botswana                 | Africa    |  1967|  53.29800|      553541|    1214.7093| NA          |
| Botswana                 | Africa    |  1972|  56.02400|      619351|    2263.6111| NA          |
| Botswana                 | Africa    |  1977|  59.31900|      781472|    3214.8578| NA          |
| Botswana                 | Africa    |  1982|  61.48400|      970347|    4551.1421| NA          |
| Botswana                 | Africa    |  1987|  63.62200|     1151184|    6205.8839| NA          |
| Botswana                 | Africa    |  1992|  62.74500|     1342614|    7954.1116| NA          |
| Botswana                 | Africa    |  1997|  52.55600|     1536536|    8647.1423| NA          |
| Botswana                 | Africa    |  2002|  46.63400|     1630347|   11003.6051| NA          |
| Botswana                 | Africa    |  2007|  50.72800|     1639131|   12569.8518| NA          |
| Brazil                   | Americas  |  1952|  50.91700|    56602560|    2108.9444| Brasilia    |
| Brazil                   | Americas  |  1957|  53.28500|    65551171|    2487.3660| Brasilia    |
| Brazil                   | Americas  |  1962|  55.66500|    76039390|    3336.5858| Brasilia    |
| Brazil                   | Americas  |  1967|  57.63200|    88049823|    3429.8644| Brasilia    |
| Brazil                   | Americas  |  1972|  59.50400|   100840058|    4985.7115| Brasilia    |
| Brazil                   | Americas  |  1977|  61.48900|   114313951|    6660.1187| Brasilia    |
| Brazil                   | Americas  |  1982|  63.33600|   128962939|    7030.8359| Brasilia    |
| Brazil                   | Americas  |  1987|  65.20500|   142938076|    7807.0958| Brasilia    |
| Brazil                   | Americas  |  1992|  67.05700|   155975974|    6950.2830| Brasilia    |
| Brazil                   | Americas  |  1997|  69.38800|   168546719|    7957.9808| Brasilia    |
| Brazil                   | Americas  |  2002|  71.00600|   179914212|    8131.2128| Brasilia    |
| Brazil                   | Americas  |  2007|  72.39000|   190010647|    9065.8008| Brasilia    |
| Bulgaria                 | Europe    |  1952|  59.60000|     7274900|    2444.2866| NA          |
| Bulgaria                 | Europe    |  1957|  66.61000|     7651254|    3008.6707| NA          |
| Bulgaria                 | Europe    |  1962|  69.51000|     8012946|    4254.3378| NA          |
| Bulgaria                 | Europe    |  1967|  70.42000|     8310226|    5577.0028| NA          |
| Bulgaria                 | Europe    |  1972|  70.90000|     8576200|    6597.4944| NA          |
| Bulgaria                 | Europe    |  1977|  70.81000|     8797022|    7612.2404| NA          |
| Bulgaria                 | Europe    |  1982|  71.08000|     8892098|    8224.1916| NA          |
| Bulgaria                 | Europe    |  1987|  71.34000|     8971958|    8239.8548| NA          |
| Bulgaria                 | Europe    |  1992|  71.19000|     8658506|    6302.6234| NA          |
| Bulgaria                 | Europe    |  1997|  70.32000|     8066057|    5970.3888| NA          |
| Bulgaria                 | Europe    |  2002|  72.14000|     7661799|    7696.7777| NA          |
| Bulgaria                 | Europe    |  2007|  73.00500|     7322858|   10680.7928| NA          |
| Burkina Faso             | Africa    |  1952|  31.97500|     4469979|     543.2552| NA          |
| Burkina Faso             | Africa    |  1957|  34.90600|     4713416|     617.1835| NA          |
| Burkina Faso             | Africa    |  1962|  37.81400|     4919632|     722.5120| NA          |
| Burkina Faso             | Africa    |  1967|  40.69700|     5127935|     794.8266| NA          |
| Burkina Faso             | Africa    |  1972|  43.59100|     5433886|     854.7360| NA          |
| Burkina Faso             | Africa    |  1977|  46.13700|     5889574|     743.3870| NA          |
| Burkina Faso             | Africa    |  1982|  48.12200|     6634596|     807.1986| NA          |
| Burkina Faso             | Africa    |  1987|  49.55700|     7586551|     912.0631| NA          |
| Burkina Faso             | Africa    |  1992|  50.26000|     8878303|     931.7528| NA          |
| Burkina Faso             | Africa    |  1997|  50.32400|    10352843|     946.2950| NA          |
| Burkina Faso             | Africa    |  2002|  50.65000|    12251209|    1037.6452| NA          |
| Burkina Faso             | Africa    |  2007|  52.29500|    14326203|    1217.0330| NA          |
| Burundi                  | Africa    |  1952|  39.03100|     2445618|     339.2965| NA          |
| Burundi                  | Africa    |  1957|  40.53300|     2667518|     379.5646| NA          |
| Burundi                  | Africa    |  1962|  42.04500|     2961915|     355.2032| NA          |
| Burundi                  | Africa    |  1967|  43.54800|     3330989|     412.9775| NA          |
| Burundi                  | Africa    |  1972|  44.05700|     3529983|     464.0995| NA          |
| Burundi                  | Africa    |  1977|  45.91000|     3834415|     556.1033| NA          |
| Burundi                  | Africa    |  1982|  47.47100|     4580410|     559.6032| NA          |
| Burundi                  | Africa    |  1987|  48.21100|     5126023|     621.8188| NA          |
| Burundi                  | Africa    |  1992|  44.73600|     5809236|     631.6999| NA          |
| Burundi                  | Africa    |  1997|  45.32600|     6121610|     463.1151| NA          |
| Burundi                  | Africa    |  2002|  47.36000|     7021078|     446.4035| NA          |
| Burundi                  | Africa    |  2007|  49.58000|     8390505|     430.0707| NA          |
| Cambodia                 | Asia      |  1952|  39.41700|     4693836|     368.4693| NA          |
| Cambodia                 | Asia      |  1957|  41.36600|     5322536|     434.0383| NA          |
| Cambodia                 | Asia      |  1962|  43.41500|     6083619|     496.9136| NA          |
| Cambodia                 | Asia      |  1967|  45.41500|     6960067|     523.4323| NA          |
| Cambodia                 | Asia      |  1972|  40.31700|     7450606|     421.6240| NA          |
| Cambodia                 | Asia      |  1977|  31.22000|     6978607|     524.9722| NA          |
| Cambodia                 | Asia      |  1982|  50.95700|     7272485|     624.4755| NA          |
| Cambodia                 | Asia      |  1987|  53.91400|     8371791|     683.8956| NA          |
| Cambodia                 | Asia      |  1992|  55.80300|    10150094|     682.3032| NA          |
| Cambodia                 | Asia      |  1997|  56.53400|    11782962|     734.2852| NA          |
| Cambodia                 | Asia      |  2002|  56.75200|    12926707|     896.2260| NA          |
| Cambodia                 | Asia      |  2007|  59.72300|    14131858|    1713.7787| NA          |
| Cameroon                 | Africa    |  1952|  38.52300|     5009067|    1172.6677| NA          |
| Cameroon                 | Africa    |  1957|  40.42800|     5359923|    1313.0481| NA          |
| Cameroon                 | Africa    |  1962|  42.64300|     5793633|    1399.6074| NA          |
| Cameroon                 | Africa    |  1967|  44.79900|     6335506|    1508.4531| NA          |
| Cameroon                 | Africa    |  1972|  47.04900|     7021028|    1684.1465| NA          |
| Cameroon                 | Africa    |  1977|  49.35500|     7959865|    1783.4329| NA          |
| Cameroon                 | Africa    |  1982|  52.96100|     9250831|    2367.9833| NA          |
| Cameroon                 | Africa    |  1987|  54.98500|    10780667|    2602.6642| NA          |
| Cameroon                 | Africa    |  1992|  54.31400|    12467171|    1793.1633| NA          |
| Cameroon                 | Africa    |  1997|  52.19900|    14195809|    1694.3375| NA          |
| Cameroon                 | Africa    |  2002|  49.85600|    15929988|    1934.0114| NA          |
| Cameroon                 | Africa    |  2007|  50.43000|    17696293|    2042.0952| NA          |
| Canada                   | Americas  |  1952|  68.75000|    14785584|   11367.1611| Ottawa      |
| Canada                   | Americas  |  1957|  69.96000|    17010154|   12489.9501| Ottawa      |
| Canada                   | Americas  |  1962|  71.30000|    18985849|   13462.4855| Ottawa      |
| Canada                   | Americas  |  1967|  72.13000|    20819767|   16076.5880| Ottawa      |
| Canada                   | Americas  |  1972|  72.88000|    22284500|   18970.5709| Ottawa      |
| Canada                   | Americas  |  1977|  74.21000|    23796400|   22090.8831| Ottawa      |
| Canada                   | Americas  |  1982|  75.76000|    25201900|   22898.7921| Ottawa      |
| Canada                   | Americas  |  1987|  76.86000|    26549700|   26626.5150| Ottawa      |
| Canada                   | Americas  |  1992|  77.95000|    28523502|   26342.8843| Ottawa      |
| Canada                   | Americas  |  1997|  78.61000|    30305843|   28954.9259| Ottawa      |
| Canada                   | Americas  |  2002|  79.77000|    31902268|   33328.9651| Ottawa      |
| Canada                   | Americas  |  2007|  80.65300|    33390141|   36319.2350| Ottawa      |
| Central African Republic | Africa    |  1952|  35.46300|     1291695|    1071.3107| NA          |
| Central African Republic | Africa    |  1957|  37.46400|     1392284|    1190.8443| NA          |
| Central African Republic | Africa    |  1962|  39.47500|     1523478|    1193.0688| NA          |
| Central African Republic | Africa    |  1967|  41.47800|     1733638|    1136.0566| NA          |
| Central African Republic | Africa    |  1972|  43.45700|     1927260|    1070.0133| NA          |
| Central African Republic | Africa    |  1977|  46.77500|     2167533|    1109.3743| NA          |
| Central African Republic | Africa    |  1982|  48.29500|     2476971|     956.7530| NA          |
| Central African Republic | Africa    |  1987|  50.48500|     2840009|     844.8764| NA          |
| Central African Republic | Africa    |  1992|  49.39600|     3265124|     747.9055| NA          |
| Central African Republic | Africa    |  1997|  46.06600|     3696513|     740.5063| NA          |
| Central African Republic | Africa    |  2002|  43.30800|     4048013|     738.6906| NA          |
| Central African Republic | Africa    |  2007|  44.74100|     4369038|     706.0165| NA          |
| Chad                     | Africa    |  1952|  38.09200|     2682462|    1178.6659| NA          |
| Chad                     | Africa    |  1957|  39.88100|     2894855|    1308.4956| NA          |
| Chad                     | Africa    |  1962|  41.71600|     3150417|    1389.8176| NA          |
| Chad                     | Africa    |  1967|  43.60100|     3495967|    1196.8106| NA          |
| Chad                     | Africa    |  1972|  45.56900|     3899068|    1104.1040| NA          |
| Chad                     | Africa    |  1977|  47.38300|     4388260|    1133.9850| NA          |
| Chad                     | Africa    |  1982|  49.51700|     4875118|     797.9081| NA          |
| Chad                     | Africa    |  1987|  51.05100|     5498955|     952.3861| NA          |
| Chad                     | Africa    |  1992|  51.72400|     6429417|    1058.0643| NA          |
| Chad                     | Africa    |  1997|  51.57300|     7562011|    1004.9614| NA          |
| Chad                     | Africa    |  2002|  50.52500|     8835739|    1156.1819| NA          |
| Chad                     | Africa    |  2007|  50.65100|    10238807|    1704.0637| NA          |
| Chile                    | Americas  |  1952|  54.74500|     6377619|    3939.9788| NA          |
| Chile                    | Americas  |  1957|  56.07400|     7048426|    4315.6227| NA          |
| Chile                    | Americas  |  1962|  57.92400|     7961258|    4519.0943| NA          |
| Chile                    | Americas  |  1967|  60.52300|     8858908|    5106.6543| NA          |
| Chile                    | Americas  |  1972|  63.44100|     9717524|    5494.0244| NA          |
| Chile                    | Americas  |  1977|  67.05200|    10599793|    4756.7638| NA          |
| Chile                    | Americas  |  1982|  70.56500|    11487112|    5095.6657| NA          |
| Chile                    | Americas  |  1987|  72.49200|    12463354|    5547.0638| NA          |
| Chile                    | Americas  |  1992|  74.12600|    13572994|    7596.1260| NA          |
| Chile                    | Americas  |  1997|  75.81600|    14599929|   10118.0532| NA          |
| Chile                    | Americas  |  2002|  77.86000|    15497046|   10778.7838| NA          |
| Chile                    | Americas  |  2007|  78.55300|    16284741|   13171.6388| NA          |
| China                    | Asia      |  1952|  44.00000|   556263527|     400.4486| Beijing     |
| China                    | Asia      |  1957|  50.54896|   637408000|     575.9870| Beijing     |
| China                    | Asia      |  1962|  44.50136|   665770000|     487.6740| Beijing     |
| China                    | Asia      |  1967|  58.38112|   754550000|     612.7057| Beijing     |
| China                    | Asia      |  1972|  63.11888|   862030000|     676.9001| Beijing     |
| China                    | Asia      |  1977|  63.96736|   943455000|     741.2375| Beijing     |
| China                    | Asia      |  1982|  65.52500|  1000281000|     962.4214| Beijing     |
| China                    | Asia      |  1987|  67.27400|  1084035000|    1378.9040| Beijing     |
| China                    | Asia      |  1992|  68.69000|  1164970000|    1655.7842| Beijing     |
| China                    | Asia      |  1997|  70.42600|  1230075000|    2289.2341| Beijing     |
| China                    | Asia      |  2002|  72.02800|  1280400000|    3119.2809| Beijing     |
| China                    | Asia      |  2007|  72.96100|  1318683096|    4959.1149| Beijing     |
| Colombia                 | Americas  |  1952|  50.64300|    12350771|    2144.1151| NA          |
| Colombia                 | Americas  |  1957|  55.11800|    14485993|    2323.8056| NA          |
| Colombia                 | Americas  |  1962|  57.86300|    17009885|    2492.3511| NA          |
| Colombia                 | Americas  |  1967|  59.96300|    19764027|    2678.7298| NA          |
| Colombia                 | Americas  |  1972|  61.62300|    22542890|    3264.6600| NA          |
| Colombia                 | Americas  |  1977|  63.83700|    25094412|    3815.8079| NA          |
| Colombia                 | Americas  |  1982|  66.65300|    27764644|    4397.5757| NA          |
| Colombia                 | Americas  |  1987|  67.76800|    30964245|    4903.2191| NA          |
| Colombia                 | Americas  |  1992|  68.42100|    34202721|    5444.6486| NA          |
| Colombia                 | Americas  |  1997|  70.31300|    37657830|    6117.3617| NA          |
| Colombia                 | Americas  |  2002|  71.68200|    41008227|    5755.2600| NA          |
| Colombia                 | Americas  |  2007|  72.88900|    44227550|    7006.5804| NA          |
| Comoros                  | Africa    |  1952|  40.71500|      153936|    1102.9909| NA          |
| Comoros                  | Africa    |  1957|  42.46000|      170928|    1211.1485| NA          |
| Comoros                  | Africa    |  1962|  44.46700|      191689|    1406.6483| NA          |
| Comoros                  | Africa    |  1967|  46.47200|      217378|    1876.0296| NA          |
| Comoros                  | Africa    |  1972|  48.94400|      250027|    1937.5777| NA          |
| Comoros                  | Africa    |  1977|  50.93900|      304739|    1172.6030| NA          |
| Comoros                  | Africa    |  1982|  52.93300|      348643|    1267.1001| NA          |
| Comoros                  | Africa    |  1987|  54.92600|      395114|    1315.9808| NA          |
| Comoros                  | Africa    |  1992|  57.93900|      454429|    1246.9074| NA          |
| Comoros                  | Africa    |  1997|  60.66000|      527982|    1173.6182| NA          |
| Comoros                  | Africa    |  2002|  62.97400|      614382|    1075.8116| NA          |
| Comoros                  | Africa    |  2007|  65.15200|      710960|     986.1479| NA          |
| Congo, Dem. Rep.         | Africa    |  1952|  39.14300|    14100005|     780.5423| NA          |
| Congo, Dem. Rep.         | Africa    |  1957|  40.65200|    15577932|     905.8602| NA          |
| Congo, Dem. Rep.         | Africa    |  1962|  42.12200|    17486434|     896.3146| NA          |
| Congo, Dem. Rep.         | Africa    |  1967|  44.05600|    19941073|     861.5932| NA          |
| Congo, Dem. Rep.         | Africa    |  1972|  45.98900|    23007669|     904.8961| NA          |
| Congo, Dem. Rep.         | Africa    |  1977|  47.80400|    26480870|     795.7573| NA          |
| Congo, Dem. Rep.         | Africa    |  1982|  47.78400|    30646495|     673.7478| NA          |
| Congo, Dem. Rep.         | Africa    |  1987|  47.41200|    35481645|     672.7748| NA          |
| Congo, Dem. Rep.         | Africa    |  1992|  45.54800|    41672143|     457.7192| NA          |
| Congo, Dem. Rep.         | Africa    |  1997|  42.58700|    47798986|     312.1884| NA          |
| Congo, Dem. Rep.         | Africa    |  2002|  44.96600|    55379852|     241.1659| NA          |
| Congo, Dem. Rep.         | Africa    |  2007|  46.46200|    64606759|     277.5519| NA          |
| Congo, Rep.              | Africa    |  1952|  42.11100|      854885|    2125.6214| NA          |
| Congo, Rep.              | Africa    |  1957|  45.05300|      940458|    2315.0566| NA          |
| Congo, Rep.              | Africa    |  1962|  48.43500|     1047924|    2464.7832| NA          |
| Congo, Rep.              | Africa    |  1967|  52.04000|     1179760|    2677.9396| NA          |
| Congo, Rep.              | Africa    |  1972|  54.90700|     1340458|    3213.1527| NA          |
| Congo, Rep.              | Africa    |  1977|  55.62500|     1536769|    3259.1790| NA          |
| Congo, Rep.              | Africa    |  1982|  56.69500|     1774735|    4879.5075| NA          |
| Congo, Rep.              | Africa    |  1987|  57.47000|     2064095|    4201.1949| NA          |
| Congo, Rep.              | Africa    |  1992|  56.43300|     2409073|    4016.2395| NA          |
| Congo, Rep.              | Africa    |  1997|  52.96200|     2800947|    3484.1644| NA          |
| Congo, Rep.              | Africa    |  2002|  52.97000|     3328795|    3484.0620| NA          |
| Congo, Rep.              | Africa    |  2007|  55.32200|     3800610|    3632.5578| NA          |
| Costa Rica               | Americas  |  1952|  57.20600|      926317|    2627.0095| NA          |
| Costa Rica               | Americas  |  1957|  60.02600|     1112300|    2990.0108| NA          |
| Costa Rica               | Americas  |  1962|  62.84200|     1345187|    3460.9370| NA          |
| Costa Rica               | Americas  |  1967|  65.42400|     1588717|    4161.7278| NA          |
| Costa Rica               | Americas  |  1972|  67.84900|     1834796|    5118.1469| NA          |
| Costa Rica               | Americas  |  1977|  70.75000|     2108457|    5926.8770| NA          |
| Costa Rica               | Americas  |  1982|  73.45000|     2424367|    5262.7348| NA          |
| Costa Rica               | Americas  |  1987|  74.75200|     2799811|    5629.9153| NA          |
| Costa Rica               | Americas  |  1992|  75.71300|     3173216|    6160.4163| NA          |
| Costa Rica               | Americas  |  1997|  77.26000|     3518107|    6677.0453| NA          |
| Costa Rica               | Americas  |  2002|  78.12300|     3834934|    7723.4472| NA          |
| Costa Rica               | Americas  |  2007|  78.78200|     4133884|    9645.0614| NA          |
| Cote d'Ivoire            | Africa    |  1952|  40.47700|     2977019|    1388.5947| NA          |
| Cote d'Ivoire            | Africa    |  1957|  42.46900|     3300000|    1500.8959| NA          |
| Cote d'Ivoire            | Africa    |  1962|  44.93000|     3832408|    1728.8694| NA          |
| Cote d'Ivoire            | Africa    |  1967|  47.35000|     4744870|    2052.0505| NA          |
| Cote d'Ivoire            | Africa    |  1972|  49.80100|     6071696|    2378.2011| NA          |
| Cote d'Ivoire            | Africa    |  1977|  52.37400|     7459574|    2517.7365| NA          |
| Cote d'Ivoire            | Africa    |  1982|  53.98300|     9025951|    2602.7102| NA          |
| Cote d'Ivoire            | Africa    |  1987|  54.65500|    10761098|    2156.9561| NA          |
| Cote d'Ivoire            | Africa    |  1992|  52.04400|    12772596|    1648.0738| NA          |
| Cote d'Ivoire            | Africa    |  1997|  47.99100|    14625967|    1786.2654| NA          |
| Cote d'Ivoire            | Africa    |  2002|  46.83200|    16252726|    1648.8008| NA          |
| Cote d'Ivoire            | Africa    |  2007|  48.32800|    18013409|    1544.7501| NA          |
| Croatia                  | Europe    |  1952|  61.21000|     3882229|    3119.2365| NA          |
| Croatia                  | Europe    |  1957|  64.77000|     3991242|    4338.2316| NA          |
| Croatia                  | Europe    |  1962|  67.13000|     4076557|    5477.8900| NA          |
| Croatia                  | Europe    |  1967|  68.50000|     4174366|    6960.2979| NA          |
| Croatia                  | Europe    |  1972|  69.61000|     4225310|    9164.0901| NA          |
| Croatia                  | Europe    |  1977|  70.64000|     4318673|   11305.3852| NA          |
| Croatia                  | Europe    |  1982|  70.46000|     4413368|   13221.8218| NA          |
| Croatia                  | Europe    |  1987|  71.52000|     4484310|   13822.5839| NA          |
| Croatia                  | Europe    |  1992|  72.52700|     4494013|    8447.7949| NA          |
| Croatia                  | Europe    |  1997|  73.68000|     4444595|    9875.6045| NA          |
| Croatia                  | Europe    |  2002|  74.87600|     4481020|   11628.3890| NA          |
| Croatia                  | Europe    |  2007|  75.74800|     4493312|   14619.2227| NA          |
| Cuba                     | Americas  |  1952|  59.42100|     6007797|    5586.5388| NA          |
| Cuba                     | Americas  |  1957|  62.32500|     6640752|    6092.1744| NA          |
| Cuba                     | Americas  |  1962|  65.24600|     7254373|    5180.7559| NA          |
| Cuba                     | Americas  |  1967|  68.29000|     8139332|    5690.2680| NA          |
| Cuba                     | Americas  |  1972|  70.72300|     8831348|    5305.4453| NA          |
| Cuba                     | Americas  |  1977|  72.64900|     9537988|    6380.4950| NA          |
| Cuba                     | Americas  |  1982|  73.71700|     9789224|    7316.9181| NA          |
| Cuba                     | Americas  |  1987|  74.17400|    10239839|    7532.9248| NA          |
| Cuba                     | Americas  |  1992|  74.41400|    10723260|    5592.8440| NA          |
| Cuba                     | Americas  |  1997|  76.15100|    10983007|    5431.9904| NA          |
| Cuba                     | Americas  |  2002|  77.15800|    11226999|    6340.6467| NA          |
| Cuba                     | Americas  |  2007|  78.27300|    11416987|    8948.1029| NA          |
| Czech Republic           | Europe    |  1952|  66.87000|     9125183|    6876.1403| NA          |
| Czech Republic           | Europe    |  1957|  69.03000|     9513758|    8256.3439| NA          |
| Czech Republic           | Europe    |  1962|  69.90000|     9620282|   10136.8671| NA          |
| Czech Republic           | Europe    |  1967|  70.38000|     9835109|   11399.4449| NA          |
| Czech Republic           | Europe    |  1972|  70.29000|     9862158|   13108.4536| NA          |
| Czech Republic           | Europe    |  1977|  70.71000|    10161915|   14800.1606| NA          |
| Czech Republic           | Europe    |  1982|  70.96000|    10303704|   15377.2285| NA          |
| Czech Republic           | Europe    |  1987|  71.58000|    10311597|   16310.4434| NA          |
| Czech Republic           | Europe    |  1992|  72.40000|    10315702|   14297.0212| NA          |
| Czech Republic           | Europe    |  1997|  74.01000|    10300707|   16048.5142| NA          |
| Czech Republic           | Europe    |  2002|  75.51000|    10256295|   17596.2102| NA          |
| Czech Republic           | Europe    |  2007|  76.48600|    10228744|   22833.3085| NA          |
| Denmark                  | Europe    |  1952|  70.78000|     4334000|    9692.3852| NA          |
| Denmark                  | Europe    |  1957|  71.81000|     4487831|   11099.6593| NA          |
| Denmark                  | Europe    |  1962|  72.35000|     4646899|   13583.3135| NA          |
| Denmark                  | Europe    |  1967|  72.96000|     4838800|   15937.2112| NA          |
| Denmark                  | Europe    |  1972|  73.47000|     4991596|   18866.2072| NA          |
| Denmark                  | Europe    |  1977|  74.69000|     5088419|   20422.9015| NA          |
| Denmark                  | Europe    |  1982|  74.63000|     5117810|   21688.0405| NA          |
| Denmark                  | Europe    |  1987|  74.80000|     5127024|   25116.1758| NA          |
| Denmark                  | Europe    |  1992|  75.33000|     5171393|   26406.7399| NA          |
| Denmark                  | Europe    |  1997|  76.11000|     5283663|   29804.3457| NA          |
| Denmark                  | Europe    |  2002|  77.18000|     5374693|   32166.5001| NA          |
| Denmark                  | Europe    |  2007|  78.33200|     5468120|   35278.4187| NA          |
| Djibouti                 | Africa    |  1952|  34.81200|       63149|    2669.5295| NA          |
| Djibouti                 | Africa    |  1957|  37.32800|       71851|    2864.9691| NA          |
| Djibouti                 | Africa    |  1962|  39.69300|       89898|    3020.9893| NA          |
| Djibouti                 | Africa    |  1967|  42.07400|      127617|    3020.0505| NA          |
| Djibouti                 | Africa    |  1972|  44.36600|      178848|    3694.2124| NA          |
| Djibouti                 | Africa    |  1977|  46.51900|      228694|    3081.7610| NA          |
| Djibouti                 | Africa    |  1982|  48.81200|      305991|    2879.4681| NA          |
| Djibouti                 | Africa    |  1987|  50.04000|      311025|    2880.1026| NA          |
| Djibouti                 | Africa    |  1992|  51.60400|      384156|    2377.1562| NA          |
| Djibouti                 | Africa    |  1997|  53.15700|      417908|    1895.0170| NA          |
| Djibouti                 | Africa    |  2002|  53.37300|      447416|    1908.2609| NA          |
| Djibouti                 | Africa    |  2007|  54.79100|      496374|    2082.4816| NA          |
| Dominican Republic       | Americas  |  1952|  45.92800|     2491346|    1397.7171| NA          |
| Dominican Republic       | Americas  |  1957|  49.82800|     2923186|    1544.4030| NA          |
| Dominican Republic       | Americas  |  1962|  53.45900|     3453434|    1662.1374| NA          |
| Dominican Republic       | Americas  |  1967|  56.75100|     4049146|    1653.7230| NA          |
| Dominican Republic       | Americas  |  1972|  59.63100|     4671329|    2189.8745| NA          |
| Dominican Republic       | Americas  |  1977|  61.78800|     5302800|    2681.9889| NA          |
| Dominican Republic       | Americas  |  1982|  63.72700|     5968349|    2861.0924| NA          |
| Dominican Republic       | Americas  |  1987|  66.04600|     6655297|    2899.8422| NA          |
| Dominican Republic       | Americas  |  1992|  68.45700|     7351181|    3044.2142| NA          |
| Dominican Republic       | Americas  |  1997|  69.95700|     7992357|    3614.1013| NA          |
| Dominican Republic       | Americas  |  2002|  70.84700|     8650322|    4563.8082| NA          |
| Dominican Republic       | Americas  |  2007|  72.23500|     9319622|    6025.3748| NA          |
| Ecuador                  | Americas  |  1952|  48.35700|     3548753|    3522.1107| NA          |
| Ecuador                  | Americas  |  1957|  51.35600|     4058385|    3780.5467| NA          |
| Ecuador                  | Americas  |  1962|  54.64000|     4681707|    4086.1141| NA          |
| Ecuador                  | Americas  |  1967|  56.67800|     5432424|    4579.0742| NA          |
| Ecuador                  | Americas  |  1972|  58.79600|     6298651|    5280.9947| NA          |
| Ecuador                  | Americas  |  1977|  61.31000|     7278866|    6679.6233| NA          |
| Ecuador                  | Americas  |  1982|  64.34200|     8365850|    7213.7913| NA          |
| Ecuador                  | Americas  |  1987|  67.23100|     9545158|    6481.7770| NA          |
| Ecuador                  | Americas  |  1992|  69.61300|    10748394|    7103.7026| NA          |
| Ecuador                  | Americas  |  1997|  72.31200|    11911819|    7429.4559| NA          |
| Ecuador                  | Americas  |  2002|  74.17300|    12921234|    5773.0445| NA          |
| Ecuador                  | Americas  |  2007|  74.99400|    13755680|    6873.2623| NA          |
| Egypt                    | Africa    |  1952|  41.89300|    22223309|    1418.8224| NA          |
| Egypt                    | Africa    |  1957|  44.44400|    25009741|    1458.9153| NA          |
| Egypt                    | Africa    |  1962|  46.99200|    28173309|    1693.3359| NA          |
| Egypt                    | Africa    |  1967|  49.29300|    31681188|    1814.8807| NA          |
| Egypt                    | Africa    |  1972|  51.13700|    34807417|    2024.0081| NA          |
| Egypt                    | Africa    |  1977|  53.31900|    38783863|    2785.4936| NA          |
| Egypt                    | Africa    |  1982|  56.00600|    45681811|    3503.7296| NA          |
| Egypt                    | Africa    |  1987|  59.79700|    52799062|    3885.4607| NA          |
| Egypt                    | Africa    |  1992|  63.67400|    59402198|    3794.7552| NA          |
| Egypt                    | Africa    |  1997|  67.21700|    66134291|    4173.1818| NA          |
| Egypt                    | Africa    |  2002|  69.80600|    73312559|    4754.6044| NA          |
| Egypt                    | Africa    |  2007|  71.33800|    80264543|    5581.1810| NA          |
| El Salvador              | Americas  |  1952|  45.26200|     2042865|    3048.3029| NA          |
| El Salvador              | Americas  |  1957|  48.57000|     2355805|    3421.5232| NA          |
| El Salvador              | Americas  |  1962|  52.30700|     2747687|    3776.8036| NA          |
| El Salvador              | Americas  |  1967|  55.85500|     3232927|    4358.5954| NA          |
| El Salvador              | Americas  |  1972|  58.20700|     3790903|    4520.2460| NA          |
| El Salvador              | Americas  |  1977|  56.69600|     4282586|    5138.9224| NA          |
| El Salvador              | Americas  |  1982|  56.60400|     4474873|    4098.3442| NA          |
| El Salvador              | Americas  |  1987|  63.15400|     4842194|    4140.4421| NA          |
| El Salvador              | Americas  |  1992|  66.79800|     5274649|    4444.2317| NA          |
| El Salvador              | Americas  |  1997|  69.53500|     5783439|    5154.8255| NA          |
| El Salvador              | Americas  |  2002|  70.73400|     6353681|    5351.5687| NA          |
| El Salvador              | Americas  |  2007|  71.87800|     6939688|    5728.3535| NA          |
| Equatorial Guinea        | Africa    |  1952|  34.48200|      216964|     375.6431| NA          |
| Equatorial Guinea        | Africa    |  1957|  35.98300|      232922|     426.0964| NA          |
| Equatorial Guinea        | Africa    |  1962|  37.48500|      249220|     582.8420| NA          |
| Equatorial Guinea        | Africa    |  1967|  38.98700|      259864|     915.5960| NA          |
| Equatorial Guinea        | Africa    |  1972|  40.51600|      277603|     672.4123| NA          |
| Equatorial Guinea        | Africa    |  1977|  42.02400|      192675|     958.5668| NA          |
| Equatorial Guinea        | Africa    |  1982|  43.66200|      285483|     927.8253| NA          |
| Equatorial Guinea        | Africa    |  1987|  45.66400|      341244|     966.8968| NA          |
| Equatorial Guinea        | Africa    |  1992|  47.54500|      387838|    1132.0550| NA          |
| Equatorial Guinea        | Africa    |  1997|  48.24500|      439971|    2814.4808| NA          |
| Equatorial Guinea        | Africa    |  2002|  49.34800|      495627|    7703.4959| NA          |
| Equatorial Guinea        | Africa    |  2007|  51.57900|      551201|   12154.0897| NA          |
| Eritrea                  | Africa    |  1952|  35.92800|     1438760|     328.9406| NA          |
| Eritrea                  | Africa    |  1957|  38.04700|     1542611|     344.1619| NA          |
| Eritrea                  | Africa    |  1962|  40.15800|     1666618|     380.9958| NA          |
| Eritrea                  | Africa    |  1967|  42.18900|     1820319|     468.7950| NA          |
| Eritrea                  | Africa    |  1972|  44.14200|     2260187|     514.3242| NA          |
| Eritrea                  | Africa    |  1977|  44.53500|     2512642|     505.7538| NA          |
| Eritrea                  | Africa    |  1982|  43.89000|     2637297|     524.8758| NA          |
| Eritrea                  | Africa    |  1987|  46.45300|     2915959|     521.1341| NA          |
| Eritrea                  | Africa    |  1992|  49.99100|     3668440|     582.8585| NA          |
| Eritrea                  | Africa    |  1997|  53.37800|     4058319|     913.4708| NA          |
| Eritrea                  | Africa    |  2002|  55.24000|     4414865|     765.3500| NA          |
| Eritrea                  | Africa    |  2007|  58.04000|     4906585|     641.3695| NA          |
| Ethiopia                 | Africa    |  1952|  34.07800|    20860941|     362.1463| Addis Ababa |
| Ethiopia                 | Africa    |  1957|  36.66700|    22815614|     378.9042| Addis Ababa |
| Ethiopia                 | Africa    |  1962|  40.05900|    25145372|     419.4564| Addis Ababa |
| Ethiopia                 | Africa    |  1967|  42.11500|    27860297|     516.1186| Addis Ababa |
| Ethiopia                 | Africa    |  1972|  43.51500|    30770372|     566.2439| Addis Ababa |
| Ethiopia                 | Africa    |  1977|  44.51000|    34617799|     556.8084| Addis Ababa |
| Ethiopia                 | Africa    |  1982|  44.91600|    38111756|     577.8607| Addis Ababa |
| Ethiopia                 | Africa    |  1987|  46.68400|    42999530|     573.7413| Addis Ababa |
| Ethiopia                 | Africa    |  1992|  48.09100|    52088559|     421.3535| Addis Ababa |
| Ethiopia                 | Africa    |  1997|  49.40200|    59861301|     515.8894| Addis Ababa |
| Ethiopia                 | Africa    |  2002|  50.72500|    67946797|     530.0535| Addis Ababa |
| Ethiopia                 | Africa    |  2007|  52.94700|    76511887|     690.8056| Addis Ababa |
| Finland                  | Europe    |  1952|  66.55000|     4090500|    6424.5191| NA          |
| Finland                  | Europe    |  1957|  67.49000|     4324000|    7545.4154| NA          |
| Finland                  | Europe    |  1962|  68.75000|     4491443|    9371.8426| NA          |
| Finland                  | Europe    |  1967|  69.83000|     4605744|   10921.6363| NA          |
| Finland                  | Europe    |  1972|  70.87000|     4639657|   14358.8759| NA          |
| Finland                  | Europe    |  1977|  72.52000|     4738902|   15605.4228| NA          |
| Finland                  | Europe    |  1982|  74.55000|     4826933|   18533.1576| NA          |
| Finland                  | Europe    |  1987|  74.83000|     4931729|   21141.0122| NA          |
| Finland                  | Europe    |  1992|  75.70000|     5041039|   20647.1650| NA          |
| Finland                  | Europe    |  1997|  77.13000|     5134406|   23723.9502| NA          |
| Finland                  | Europe    |  2002|  78.37000|     5193039|   28204.5906| NA          |
| Finland                  | Europe    |  2007|  79.31300|     5238460|   33207.0844| NA          |
| France                   | Europe    |  1952|  67.41000|    42459667|    7029.8093| Paris       |
| France                   | Europe    |  1957|  68.93000|    44310863|    8662.8349| Paris       |
| France                   | Europe    |  1962|  70.51000|    47124000|   10560.4855| Paris       |
| France                   | Europe    |  1967|  71.55000|    49569000|   12999.9177| Paris       |
| France                   | Europe    |  1972|  72.38000|    51732000|   16107.1917| Paris       |
| France                   | Europe    |  1977|  73.83000|    53165019|   18292.6351| Paris       |
| France                   | Europe    |  1982|  74.89000|    54433565|   20293.8975| Paris       |
| France                   | Europe    |  1987|  76.34000|    55630100|   22066.4421| Paris       |
| France                   | Europe    |  1992|  77.46000|    57374179|   24703.7961| Paris       |
| France                   | Europe    |  1997|  78.64000|    58623428|   25889.7849| Paris       |
| France                   | Europe    |  2002|  79.59000|    59925035|   28926.0323| Paris       |
| France                   | Europe    |  2007|  80.65700|    61083916|   30470.0167| Paris       |
| Gabon                    | Africa    |  1952|  37.00300|      420702|    4293.4765| NA          |
| Gabon                    | Africa    |  1957|  38.99900|      434904|    4976.1981| NA          |
| Gabon                    | Africa    |  1962|  40.48900|      455661|    6631.4592| NA          |
| Gabon                    | Africa    |  1967|  44.59800|      489004|    8358.7620| NA          |
| Gabon                    | Africa    |  1972|  48.69000|      537977|   11401.9484| NA          |
| Gabon                    | Africa    |  1977|  52.79000|      706367|   21745.5733| NA          |
| Gabon                    | Africa    |  1982|  56.56400|      753874|   15113.3619| NA          |
| Gabon                    | Africa    |  1987|  60.19000|      880397|   11864.4084| NA          |
| Gabon                    | Africa    |  1992|  61.36600|      985739|   13522.1575| NA          |
| Gabon                    | Africa    |  1997|  60.46100|     1126189|   14722.8419| NA          |
| Gabon                    | Africa    |  2002|  56.76100|     1299304|   12521.7139| NA          |
| Gabon                    | Africa    |  2007|  56.73500|     1454867|   13206.4845| NA          |
| Gambia                   | Africa    |  1952|  30.00000|      284320|     485.2307| NA          |
| Gambia                   | Africa    |  1957|  32.06500|      323150|     520.9267| NA          |
| Gambia                   | Africa    |  1962|  33.89600|      374020|     599.6503| NA          |
| Gambia                   | Africa    |  1967|  35.85700|      439593|     734.7829| NA          |
| Gambia                   | Africa    |  1972|  38.30800|      517101|     756.0868| NA          |
| Gambia                   | Africa    |  1977|  41.84200|      608274|     884.7553| NA          |
| Gambia                   | Africa    |  1982|  45.58000|      715523|     835.8096| NA          |
| Gambia                   | Africa    |  1987|  49.26500|      848406|     611.6589| NA          |
| Gambia                   | Africa    |  1992|  52.64400|     1025384|     665.6244| NA          |
| Gambia                   | Africa    |  1997|  55.86100|     1235767|     653.7302| NA          |
| Gambia                   | Africa    |  2002|  58.04100|     1457766|     660.5856| NA          |
| Gambia                   | Africa    |  2007|  59.44800|     1688359|     752.7497| NA          |
| Germany                  | Europe    |  1952|  67.50000|    69145952|    7144.1144| NA          |
| Germany                  | Europe    |  1957|  69.10000|    71019069|   10187.8267| NA          |
| Germany                  | Europe    |  1962|  70.30000|    73739117|   12902.4629| NA          |
| Germany                  | Europe    |  1967|  70.80000|    76368453|   14745.6256| NA          |
| Germany                  | Europe    |  1972|  71.00000|    78717088|   18016.1803| NA          |
| Germany                  | Europe    |  1977|  72.50000|    78160773|   20512.9212| NA          |
| Germany                  | Europe    |  1982|  73.80000|    78335266|   22031.5327| NA          |
| Germany                  | Europe    |  1987|  74.84700|    77718298|   24639.1857| NA          |
| Germany                  | Europe    |  1992|  76.07000|    80597764|   26505.3032| NA          |
| Germany                  | Europe    |  1997|  77.34000|    82011073|   27788.8842| NA          |
| Germany                  | Europe    |  2002|  78.67000|    82350671|   30035.8020| NA          |
| Germany                  | Europe    |  2007|  79.40600|    82400996|   32170.3744| NA          |
| Ghana                    | Africa    |  1952|  43.14900|     5581001|     911.2989| NA          |
| Ghana                    | Africa    |  1957|  44.77900|     6391288|    1043.5615| NA          |
| Ghana                    | Africa    |  1962|  46.45200|     7355248|    1190.0411| NA          |
| Ghana                    | Africa    |  1967|  48.07200|     8490213|    1125.6972| NA          |
| Ghana                    | Africa    |  1972|  49.87500|     9354120|    1178.2237| NA          |
| Ghana                    | Africa    |  1977|  51.75600|    10538093|     993.2240| NA          |
| Ghana                    | Africa    |  1982|  53.74400|    11400338|     876.0326| NA          |
| Ghana                    | Africa    |  1987|  55.72900|    14168101|     847.0061| NA          |
| Ghana                    | Africa    |  1992|  57.50100|    16278738|     925.0602| NA          |
| Ghana                    | Africa    |  1997|  58.55600|    18418288|    1005.2458| NA          |
| Ghana                    | Africa    |  2002|  58.45300|    20550751|    1111.9846| NA          |
| Ghana                    | Africa    |  2007|  60.02200|    22873338|    1327.6089| NA          |
| Greece                   | Europe    |  1952|  65.86000|     7733250|    3530.6901| Athens      |
| Greece                   | Europe    |  1957|  67.86000|     8096218|    4916.2999| Athens      |
| Greece                   | Europe    |  1962|  69.51000|     8448233|    6017.1907| Athens      |
| Greece                   | Europe    |  1967|  71.00000|     8716441|    8513.0970| Athens      |
| Greece                   | Europe    |  1972|  72.34000|     8888628|   12724.8296| Athens      |
| Greece                   | Europe    |  1977|  73.68000|     9308479|   14195.5243| Athens      |
| Greece                   | Europe    |  1982|  75.24000|     9786480|   15268.4209| Athens      |
| Greece                   | Europe    |  1987|  76.67000|     9974490|   16120.5284| Athens      |
| Greece                   | Europe    |  1992|  77.03000|    10325429|   17541.4963| Athens      |
| Greece                   | Europe    |  1997|  77.86900|    10502372|   18747.6981| Athens      |
| Greece                   | Europe    |  2002|  78.25600|    10603863|   22514.2548| Athens      |
| Greece                   | Europe    |  2007|  79.48300|    10706290|   27538.4119| Athens      |
| Guatemala                | Americas  |  1952|  42.02300|     3146381|    2428.2378| NA          |
| Guatemala                | Americas  |  1957|  44.14200|     3640876|    2617.1560| NA          |
| Guatemala                | Americas  |  1962|  46.95400|     4208858|    2750.3644| NA          |
| Guatemala                | Americas  |  1967|  50.01600|     4690773|    3242.5311| NA          |
| Guatemala                | Americas  |  1972|  53.73800|     5149581|    4031.4083| NA          |
| Guatemala                | Americas  |  1977|  56.02900|     5703430|    4879.9927| NA          |
| Guatemala                | Americas  |  1982|  58.13700|     6395630|    4820.4948| NA          |
| Guatemala                | Americas  |  1987|  60.78200|     7326406|    4246.4860| NA          |
| Guatemala                | Americas  |  1992|  63.37300|     8486949|    4439.4508| NA          |
| Guatemala                | Americas  |  1997|  66.32200|     9803875|    4684.3138| NA          |
| Guatemala                | Americas  |  2002|  68.97800|    11178650|    4858.3475| NA          |
| Guatemala                | Americas  |  2007|  70.25900|    12572928|    5186.0500| NA          |
| Guinea                   | Africa    |  1952|  33.60900|     2664249|     510.1965| NA          |
| Guinea                   | Africa    |  1957|  34.55800|     2876726|     576.2670| NA          |
| Guinea                   | Africa    |  1962|  35.75300|     3140003|     686.3737| NA          |
| Guinea                   | Africa    |  1967|  37.19700|     3451418|     708.7595| NA          |
| Guinea                   | Africa    |  1972|  38.84200|     3811387|     741.6662| NA          |
| Guinea                   | Africa    |  1977|  40.76200|     4227026|     874.6859| NA          |
| Guinea                   | Africa    |  1982|  42.89100|     4710497|     857.2504| NA          |
| Guinea                   | Africa    |  1987|  45.55200|     5650262|     805.5725| NA          |
| Guinea                   | Africa    |  1992|  48.57600|     6990574|     794.3484| NA          |
| Guinea                   | Africa    |  1997|  51.45500|     8048834|     869.4498| NA          |
| Guinea                   | Africa    |  2002|  53.67600|     8807818|     945.5836| NA          |
| Guinea                   | Africa    |  2007|  56.00700|     9947814|     942.6542| NA          |
| Guinea-Bissau            | Africa    |  1952|  32.50000|      580653|     299.8503| NA          |
| Guinea-Bissau            | Africa    |  1957|  33.48900|      601095|     431.7905| NA          |
| Guinea-Bissau            | Africa    |  1962|  34.48800|      627820|     522.0344| NA          |
| Guinea-Bissau            | Africa    |  1967|  35.49200|      601287|     715.5806| NA          |
| Guinea-Bissau            | Africa    |  1972|  36.48600|      625361|     820.2246| NA          |
| Guinea-Bissau            | Africa    |  1977|  37.46500|      745228|     764.7260| NA          |
| Guinea-Bissau            | Africa    |  1982|  39.32700|      825987|     838.1240| NA          |
| Guinea-Bissau            | Africa    |  1987|  41.24500|      927524|     736.4154| NA          |
| Guinea-Bissau            | Africa    |  1992|  43.26600|     1050938|     745.5399| NA          |
| Guinea-Bissau            | Africa    |  1997|  44.87300|     1193708|     796.6645| NA          |
| Guinea-Bissau            | Africa    |  2002|  45.50400|     1332459|     575.7047| NA          |
| Guinea-Bissau            | Africa    |  2007|  46.38800|     1472041|     579.2317| NA          |
| Haiti                    | Americas  |  1952|  37.57900|     3201488|    1840.3669| NA          |
| Haiti                    | Americas  |  1957|  40.69600|     3507701|    1726.8879| NA          |
| Haiti                    | Americas  |  1962|  43.59000|     3880130|    1796.5890| NA          |
| Haiti                    | Americas  |  1967|  46.24300|     4318137|    1452.0577| NA          |
| Haiti                    | Americas  |  1972|  48.04200|     4698301|    1654.4569| NA          |
| Haiti                    | Americas  |  1977|  49.92300|     4908554|    1874.2989| NA          |
| Haiti                    | Americas  |  1982|  51.46100|     5198399|    2011.1595| NA          |
| Haiti                    | Americas  |  1987|  53.63600|     5756203|    1823.0160| NA          |
| Haiti                    | Americas  |  1992|  55.08900|     6326682|    1456.3095| NA          |
| Haiti                    | Americas  |  1997|  56.67100|     6913545|    1341.7269| NA          |
| Haiti                    | Americas  |  2002|  58.13700|     7607651|    1270.3649| NA          |
| Haiti                    | Americas  |  2007|  60.91600|     8502814|    1201.6372| NA          |
| Honduras                 | Americas  |  1952|  41.91200|     1517453|    2194.9262| NA          |
| Honduras                 | Americas  |  1957|  44.66500|     1770390|    2220.4877| NA          |
| Honduras                 | Americas  |  1962|  48.04100|     2090162|    2291.1568| NA          |
| Honduras                 | Americas  |  1967|  50.92400|     2500689|    2538.2694| NA          |
| Honduras                 | Americas  |  1972|  53.88400|     2965146|    2529.8423| NA          |
| Honduras                 | Americas  |  1977|  57.40200|     3055235|    3203.2081| NA          |
| Honduras                 | Americas  |  1982|  60.90900|     3669448|    3121.7608| NA          |
| Honduras                 | Americas  |  1987|  64.49200|     4372203|    3023.0967| NA          |
| Honduras                 | Americas  |  1992|  66.39900|     5077347|    3081.6946| NA          |
| Honduras                 | Americas  |  1997|  67.65900|     5867957|    3160.4549| NA          |
| Honduras                 | Americas  |  2002|  68.56500|     6677328|    3099.7287| NA          |
| Honduras                 | Americas  |  2007|  70.19800|     7483763|    3548.3308| NA          |
| Hong Kong, China         | Asia      |  1952|  60.96000|     2125900|    3054.4212| NA          |
| Hong Kong, China         | Asia      |  1957|  64.75000|     2736300|    3629.0765| NA          |
| Hong Kong, China         | Asia      |  1962|  67.65000|     3305200|    4692.6483| NA          |
| Hong Kong, China         | Asia      |  1967|  70.00000|     3722800|    6197.9628| NA          |
| Hong Kong, China         | Asia      |  1972|  72.00000|     4115700|    8315.9281| NA          |
| Hong Kong, China         | Asia      |  1977|  73.60000|     4583700|   11186.1413| NA          |
| Hong Kong, China         | Asia      |  1982|  75.45000|     5264500|   14560.5305| NA          |
| Hong Kong, China         | Asia      |  1987|  76.20000|     5584510|   20038.4727| NA          |
| Hong Kong, China         | Asia      |  1992|  77.60100|     5829696|   24757.6030| NA          |
| Hong Kong, China         | Asia      |  1997|  80.00000|     6495918|   28377.6322| NA          |
| Hong Kong, China         | Asia      |  2002|  81.49500|     6762476|   30209.0152| NA          |
| Hong Kong, China         | Asia      |  2007|  82.20800|     6980412|   39724.9787| NA          |
| Hungary                  | Europe    |  1952|  64.03000|     9504000|    5263.6738| NA          |
| Hungary                  | Europe    |  1957|  66.41000|     9839000|    6040.1800| NA          |
| Hungary                  | Europe    |  1962|  67.96000|    10063000|    7550.3599| NA          |
| Hungary                  | Europe    |  1967|  69.50000|    10223422|    9326.6447| NA          |
| Hungary                  | Europe    |  1972|  69.76000|    10394091|   10168.6561| NA          |
| Hungary                  | Europe    |  1977|  69.95000|    10637171|   11674.8374| NA          |
| Hungary                  | Europe    |  1982|  69.39000|    10705535|   12545.9907| NA          |
| Hungary                  | Europe    |  1987|  69.58000|    10612740|   12986.4800| NA          |
| Hungary                  | Europe    |  1992|  69.17000|    10348684|   10535.6285| NA          |
| Hungary                  | Europe    |  1997|  71.04000|    10244684|   11712.7768| NA          |
| Hungary                  | Europe    |  2002|  72.59000|    10083313|   14843.9356| NA          |
| Hungary                  | Europe    |  2007|  73.33800|     9956108|   18008.9444| NA          |
| Iceland                  | Europe    |  1952|  72.49000|      147962|    7267.6884| NA          |
| Iceland                  | Europe    |  1957|  73.47000|      165110|    9244.0014| NA          |
| Iceland                  | Europe    |  1962|  73.68000|      182053|   10350.1591| NA          |
| Iceland                  | Europe    |  1967|  73.73000|      198676|   13319.8957| NA          |
| Iceland                  | Europe    |  1972|  74.46000|      209275|   15798.0636| NA          |
| Iceland                  | Europe    |  1977|  76.11000|      221823|   19654.9625| NA          |
| Iceland                  | Europe    |  1982|  76.99000|      233997|   23269.6075| NA          |
| Iceland                  | Europe    |  1987|  77.23000|      244676|   26923.2063| NA          |
| Iceland                  | Europe    |  1992|  78.77000|      259012|   25144.3920| NA          |
| Iceland                  | Europe    |  1997|  78.95000|      271192|   28061.0997| NA          |
| Iceland                  | Europe    |  2002|  80.50000|      288030|   31163.2020| NA          |
| Iceland                  | Europe    |  2007|  81.75700|      301931|   36180.7892| NA          |
| India                    | Asia      |  1952|  37.37300|   372000000|     546.5657| NA          |
| India                    | Asia      |  1957|  40.24900|   409000000|     590.0620| NA          |
| India                    | Asia      |  1962|  43.60500|   454000000|     658.3472| NA          |
| India                    | Asia      |  1967|  47.19300|   506000000|     700.7706| NA          |
| India                    | Asia      |  1972|  50.65100|   567000000|     724.0325| NA          |
| India                    | Asia      |  1977|  54.20800|   634000000|     813.3373| NA          |
| India                    | Asia      |  1982|  56.59600|   708000000|     855.7235| NA          |
| India                    | Asia      |  1987|  58.55300|   788000000|     976.5127| NA          |
| India                    | Asia      |  1992|  60.22300|   872000000|    1164.4068| NA          |
| India                    | Asia      |  1997|  61.76500|   959000000|    1458.8174| NA          |
| India                    | Asia      |  2002|  62.87900|  1034172547|    1746.7695| NA          |
| India                    | Asia      |  2007|  64.69800|  1110396331|    2452.2104| NA          |
| Indonesia                | Asia      |  1952|  37.46800|    82052000|     749.6817| NA          |
| Indonesia                | Asia      |  1957|  39.91800|    90124000|     858.9003| NA          |
| Indonesia                | Asia      |  1962|  42.51800|    99028000|     849.2898| NA          |
| Indonesia                | Asia      |  1967|  45.96400|   109343000|     762.4318| NA          |
| Indonesia                | Asia      |  1972|  49.20300|   121282000|    1111.1079| NA          |
| Indonesia                | Asia      |  1977|  52.70200|   136725000|    1382.7021| NA          |
| Indonesia                | Asia      |  1982|  56.15900|   153343000|    1516.8730| NA          |
| Indonesia                | Asia      |  1987|  60.13700|   169276000|    1748.3570| NA          |
| Indonesia                | Asia      |  1992|  62.68100|   184816000|    2383.1409| NA          |
| Indonesia                | Asia      |  1997|  66.04100|   199278000|    3119.3356| NA          |
| Indonesia                | Asia      |  2002|  68.58800|   211060000|    2873.9129| NA          |
| Indonesia                | Asia      |  2007|  70.65000|   223547000|    3540.6516| NA          |
| Iran                     | Asia      |  1952|  44.86900|    17272000|    3035.3260| NA          |
| Iran                     | Asia      |  1957|  47.18100|    19792000|    3290.2576| NA          |
| Iran                     | Asia      |  1962|  49.32500|    22874000|    4187.3298| NA          |
| Iran                     | Asia      |  1967|  52.46900|    26538000|    5906.7318| NA          |
| Iran                     | Asia      |  1972|  55.23400|    30614000|    9613.8186| NA          |
| Iran                     | Asia      |  1977|  57.70200|    35480679|   11888.5951| NA          |
| Iran                     | Asia      |  1982|  59.62000|    43072751|    7608.3346| NA          |
| Iran                     | Asia      |  1987|  63.04000|    51889696|    6642.8814| NA          |
| Iran                     | Asia      |  1992|  65.74200|    60397973|    7235.6532| NA          |
| Iran                     | Asia      |  1997|  68.04200|    63327987|    8263.5903| NA          |
| Iran                     | Asia      |  2002|  69.45100|    66907826|    9240.7620| NA          |
| Iran                     | Asia      |  2007|  70.96400|    69453570|   11605.7145| NA          |
| Iraq                     | Asia      |  1952|  45.32000|     5441766|    4129.7661| NA          |
| Iraq                     | Asia      |  1957|  48.43700|     6248643|    6229.3336| NA          |
| Iraq                     | Asia      |  1962|  51.45700|     7240260|    8341.7378| NA          |
| Iraq                     | Asia      |  1967|  54.45900|     8519282|    8931.4598| NA          |
| Iraq                     | Asia      |  1972|  56.95000|    10061506|    9576.0376| NA          |
| Iraq                     | Asia      |  1977|  60.41300|    11882916|   14688.2351| NA          |
| Iraq                     | Asia      |  1982|  62.03800|    14173318|   14517.9071| NA          |
| Iraq                     | Asia      |  1987|  65.04400|    16543189|   11643.5727| NA          |
| Iraq                     | Asia      |  1992|  59.46100|    17861905|    3745.6407| NA          |
| Iraq                     | Asia      |  1997|  58.81100|    20775703|    3076.2398| NA          |
| Iraq                     | Asia      |  2002|  57.04600|    24001816|    4390.7173| NA          |
| Iraq                     | Asia      |  2007|  59.54500|    27499638|    4471.0619| NA          |
| Ireland                  | Europe    |  1952|  66.91000|     2952156|    5210.2803| NA          |
| Ireland                  | Europe    |  1957|  68.90000|     2878220|    5599.0779| NA          |
| Ireland                  | Europe    |  1962|  70.29000|     2830000|    6631.5973| NA          |
| Ireland                  | Europe    |  1967|  71.08000|     2900100|    7655.5690| NA          |
| Ireland                  | Europe    |  1972|  71.28000|     3024400|    9530.7729| NA          |
| Ireland                  | Europe    |  1977|  72.03000|     3271900|   11150.9811| NA          |
| Ireland                  | Europe    |  1982|  73.10000|     3480000|   12618.3214| NA          |
| Ireland                  | Europe    |  1987|  74.36000|     3539900|   13872.8665| NA          |
| Ireland                  | Europe    |  1992|  75.46700|     3557761|   17558.8155| NA          |
| Ireland                  | Europe    |  1997|  76.12200|     3667233|   24521.9471| NA          |
| Ireland                  | Europe    |  2002|  77.78300|     3879155|   34077.0494| NA          |
| Ireland                  | Europe    |  2007|  78.88500|     4109086|   40675.9964| NA          |
| Israel                   | Asia      |  1952|  65.39000|     1620914|    4086.5221| NA          |
| Israel                   | Asia      |  1957|  67.84000|     1944401|    5385.2785| NA          |
| Israel                   | Asia      |  1962|  69.39000|     2310904|    7105.6307| NA          |
| Israel                   | Asia      |  1967|  70.75000|     2693585|    8393.7414| NA          |
| Israel                   | Asia      |  1972|  71.63000|     3095893|   12786.9322| NA          |
| Israel                   | Asia      |  1977|  73.06000|     3495918|   13306.6192| NA          |
| Israel                   | Asia      |  1982|  74.45000|     3858421|   15367.0292| NA          |
| Israel                   | Asia      |  1987|  75.60000|     4203148|   17122.4799| NA          |
| Israel                   | Asia      |  1992|  76.93000|     4936550|   18051.5225| NA          |
| Israel                   | Asia      |  1997|  78.26900|     5531387|   20896.6092| NA          |
| Israel                   | Asia      |  2002|  79.69600|     6029529|   21905.5951| NA          |
| Israel                   | Asia      |  2007|  80.74500|     6426679|   25523.2771| NA          |
| Italy                    | Europe    |  1952|  65.94000|    47666000|    4931.4042| NA          |
| Italy                    | Europe    |  1957|  67.81000|    49182000|    6248.6562| NA          |
| Italy                    | Europe    |  1962|  69.24000|    50843200|    8243.5823| NA          |
| Italy                    | Europe    |  1967|  71.06000|    52667100|   10022.4013| NA          |
| Italy                    | Europe    |  1972|  72.19000|    54365564|   12269.2738| NA          |
| Italy                    | Europe    |  1977|  73.48000|    56059245|   14255.9847| NA          |
| Italy                    | Europe    |  1982|  74.98000|    56535636|   16537.4835| NA          |
| Italy                    | Europe    |  1987|  76.42000|    56729703|   19207.2348| NA          |
| Italy                    | Europe    |  1992|  77.44000|    56840847|   22013.6449| NA          |
| Italy                    | Europe    |  1997|  78.82000|    57479469|   24675.0245| NA          |
| Italy                    | Europe    |  2002|  80.24000|    57926999|   27968.0982| NA          |
| Italy                    | Europe    |  2007|  80.54600|    58147733|   28569.7197| NA          |
| Jamaica                  | Americas  |  1952|  58.53000|     1426095|    2898.5309| NA          |
| Jamaica                  | Americas  |  1957|  62.61000|     1535090|    4756.5258| NA          |
| Jamaica                  | Americas  |  1962|  65.61000|     1665128|    5246.1075| NA          |
| Jamaica                  | Americas  |  1967|  67.51000|     1861096|    6124.7035| NA          |
| Jamaica                  | Americas  |  1972|  69.00000|     1997616|    7433.8893| NA          |
| Jamaica                  | Americas  |  1977|  70.11000|     2156814|    6650.1956| NA          |
| Jamaica                  | Americas  |  1982|  71.21000|     2298309|    6068.0513| NA          |
| Jamaica                  | Americas  |  1987|  71.77000|     2326606|    6351.2375| NA          |
| Jamaica                  | Americas  |  1992|  71.76600|     2378618|    7404.9237| NA          |
| Jamaica                  | Americas  |  1997|  72.26200|     2531311|    7121.9247| NA          |
| Jamaica                  | Americas  |  2002|  72.04700|     2664659|    6994.7749| NA          |
| Jamaica                  | Americas  |  2007|  72.56700|     2780132|    7320.8803| NA          |
| Japan                    | Asia      |  1952|  63.03000|    86459025|    3216.9563| Tokyo       |
| Japan                    | Asia      |  1957|  65.50000|    91563009|    4317.6944| Tokyo       |
| Japan                    | Asia      |  1962|  68.73000|    95831757|    6576.6495| Tokyo       |
| Japan                    | Asia      |  1967|  71.43000|   100825279|    9847.7886| Tokyo       |
| Japan                    | Asia      |  1972|  73.42000|   107188273|   14778.7864| Tokyo       |
| Japan                    | Asia      |  1977|  75.38000|   113872473|   16610.3770| Tokyo       |
| Japan                    | Asia      |  1982|  77.11000|   118454974|   19384.1057| Tokyo       |
| Japan                    | Asia      |  1987|  78.67000|   122091325|   22375.9419| Tokyo       |
| Japan                    | Asia      |  1992|  79.36000|   124329269|   26824.8951| Tokyo       |
| Japan                    | Asia      |  1997|  80.69000|   125956499|   28816.5850| Tokyo       |
| Japan                    | Asia      |  2002|  82.00000|   127065841|   28604.5919| Tokyo       |
| Japan                    | Asia      |  2007|  82.60300|   127467972|   31656.0681| Tokyo       |
| Jordan                   | Asia      |  1952|  43.15800|      607914|    1546.9078| NA          |
| Jordan                   | Asia      |  1957|  45.66900|      746559|    1886.0806| NA          |
| Jordan                   | Asia      |  1962|  48.12600|      933559|    2348.0092| NA          |
| Jordan                   | Asia      |  1967|  51.62900|     1255058|    2741.7963| NA          |
| Jordan                   | Asia      |  1972|  56.52800|     1613551|    2110.8563| NA          |
| Jordan                   | Asia      |  1977|  61.13400|     1937652|    2852.3516| NA          |
| Jordan                   | Asia      |  1982|  63.73900|     2347031|    4161.4160| NA          |
| Jordan                   | Asia      |  1987|  65.86900|     2820042|    4448.6799| NA          |
| Jordan                   | Asia      |  1992|  68.01500|     3867409|    3431.5936| NA          |
| Jordan                   | Asia      |  1997|  69.77200|     4526235|    3645.3796| NA          |
| Jordan                   | Asia      |  2002|  71.26300|     5307470|    3844.9172| NA          |
| Jordan                   | Asia      |  2007|  72.53500|     6053193|    4519.4612| NA          |
| Kenya                    | Africa    |  1952|  42.27000|     6464046|     853.5409| NA          |
| Kenya                    | Africa    |  1957|  44.68600|     7454779|     944.4383| NA          |
| Kenya                    | Africa    |  1962|  47.94900|     8678557|     896.9664| NA          |
| Kenya                    | Africa    |  1967|  50.65400|    10191512|    1056.7365| NA          |
| Kenya                    | Africa    |  1972|  53.55900|    12044785|    1222.3600| NA          |
| Kenya                    | Africa    |  1977|  56.15500|    14500404|    1267.6132| NA          |
| Kenya                    | Africa    |  1982|  58.76600|    17661452|    1348.2258| NA          |
| Kenya                    | Africa    |  1987|  59.33900|    21198082|    1361.9369| NA          |
| Kenya                    | Africa    |  1992|  59.28500|    25020539|    1341.9217| NA          |
| Kenya                    | Africa    |  1997|  54.40700|    28263827|    1360.4850| NA          |
| Kenya                    | Africa    |  2002|  50.99200|    31386842|    1287.5147| NA          |
| Kenya                    | Africa    |  2007|  54.11000|    35610177|    1463.2493| NA          |
| Korea, Dem. Rep.         | Asia      |  1952|  50.05600|     8865488|    1088.2778| NA          |
| Korea, Dem. Rep.         | Asia      |  1957|  54.08100|     9411381|    1571.1347| NA          |
| Korea, Dem. Rep.         | Asia      |  1962|  56.65600|    10917494|    1621.6936| NA          |
| Korea, Dem. Rep.         | Asia      |  1967|  59.94200|    12617009|    2143.5406| NA          |
| Korea, Dem. Rep.         | Asia      |  1972|  63.98300|    14781241|    3701.6215| NA          |
| Korea, Dem. Rep.         | Asia      |  1977|  67.15900|    16325320|    4106.3012| NA          |
| Korea, Dem. Rep.         | Asia      |  1982|  69.10000|    17647518|    4106.5253| NA          |
| Korea, Dem. Rep.         | Asia      |  1987|  70.64700|    19067554|    4106.4923| NA          |
| Korea, Dem. Rep.         | Asia      |  1992|  69.97800|    20711375|    3726.0635| NA          |
| Korea, Dem. Rep.         | Asia      |  1997|  67.72700|    21585105|    1690.7568| NA          |
| Korea, Dem. Rep.         | Asia      |  2002|  66.66200|    22215365|    1646.7582| NA          |
| Korea, Dem. Rep.         | Asia      |  2007|  67.29700|    23301725|    1593.0655| NA          |
| Korea, Rep.              | Asia      |  1952|  47.45300|    20947571|    1030.5922| NA          |
| Korea, Rep.              | Asia      |  1957|  52.68100|    22611552|    1487.5935| NA          |
| Korea, Rep.              | Asia      |  1962|  55.29200|    26420307|    1536.3444| NA          |
| Korea, Rep.              | Asia      |  1967|  57.71600|    30131000|    2029.2281| NA          |
| Korea, Rep.              | Asia      |  1972|  62.61200|    33505000|    3030.8767| NA          |
| Korea, Rep.              | Asia      |  1977|  64.76600|    36436000|    4657.2210| NA          |
| Korea, Rep.              | Asia      |  1982|  67.12300|    39326000|    5622.9425| NA          |
| Korea, Rep.              | Asia      |  1987|  69.81000|    41622000|    8533.0888| NA          |
| Korea, Rep.              | Asia      |  1992|  72.24400|    43805450|   12104.2787| NA          |
| Korea, Rep.              | Asia      |  1997|  74.64700|    46173816|   15993.5280| NA          |
| Korea, Rep.              | Asia      |  2002|  77.04500|    47969150|   19233.9882| NA          |
| Korea, Rep.              | Asia      |  2007|  78.62300|    49044790|   23348.1397| NA          |
| Kuwait                   | Asia      |  1952|  55.56500|      160000|  108382.3529| NA          |
| Kuwait                   | Asia      |  1957|  58.03300|      212846|  113523.1329| NA          |
| Kuwait                   | Asia      |  1962|  60.47000|      358266|   95458.1118| NA          |
| Kuwait                   | Asia      |  1967|  64.62400|      575003|   80894.8833| NA          |
| Kuwait                   | Asia      |  1972|  67.71200|      841934|  109347.8670| NA          |
| Kuwait                   | Asia      |  1977|  69.34300|     1140357|   59265.4771| NA          |
| Kuwait                   | Asia      |  1982|  71.30900|     1497494|   31354.0357| NA          |
| Kuwait                   | Asia      |  1987|  74.17400|     1891487|   28118.4300| NA          |
| Kuwait                   | Asia      |  1992|  75.19000|     1418095|   34932.9196| NA          |
| Kuwait                   | Asia      |  1997|  76.15600|     1765345|   40300.6200| NA          |
| Kuwait                   | Asia      |  2002|  76.90400|     2111561|   35110.1057| NA          |
| Kuwait                   | Asia      |  2007|  77.58800|     2505559|   47306.9898| NA          |
| Lebanon                  | Asia      |  1952|  55.92800|     1439529|    4834.8041| NA          |
| Lebanon                  | Asia      |  1957|  59.48900|     1647412|    6089.7869| NA          |
| Lebanon                  | Asia      |  1962|  62.09400|     1886848|    5714.5606| NA          |
| Lebanon                  | Asia      |  1967|  63.87000|     2186894|    6006.9830| NA          |
| Lebanon                  | Asia      |  1972|  65.42100|     2680018|    7486.3843| NA          |
| Lebanon                  | Asia      |  1977|  66.09900|     3115787|    8659.6968| NA          |
| Lebanon                  | Asia      |  1982|  66.98300|     3086876|    7640.5195| NA          |
| Lebanon                  | Asia      |  1987|  67.92600|     3089353|    5377.0913| NA          |
| Lebanon                  | Asia      |  1992|  69.29200|     3219994|    6890.8069| NA          |
| Lebanon                  | Asia      |  1997|  70.26500|     3430388|    8754.9639| NA          |
| Lebanon                  | Asia      |  2002|  71.02800|     3677780|    9313.9388| NA          |
| Lebanon                  | Asia      |  2007|  71.99300|     3921278|   10461.0587| NA          |
| Lesotho                  | Africa    |  1952|  42.13800|      748747|     298.8462| NA          |
| Lesotho                  | Africa    |  1957|  45.04700|      813338|     335.9971| NA          |
| Lesotho                  | Africa    |  1962|  47.74700|      893143|     411.8006| NA          |
| Lesotho                  | Africa    |  1967|  48.49200|      996380|     498.6390| NA          |
| Lesotho                  | Africa    |  1972|  49.76700|     1116779|     496.5816| NA          |
| Lesotho                  | Africa    |  1977|  52.20800|     1251524|     745.3695| NA          |
| Lesotho                  | Africa    |  1982|  55.07800|     1411807|     797.2631| NA          |
| Lesotho                  | Africa    |  1987|  57.18000|     1599200|     773.9932| NA          |
| Lesotho                  | Africa    |  1992|  59.68500|     1803195|     977.4863| NA          |
| Lesotho                  | Africa    |  1997|  55.55800|     1982823|    1186.1480| NA          |
| Lesotho                  | Africa    |  2002|  44.59300|     2046772|    1275.1846| NA          |
| Lesotho                  | Africa    |  2007|  42.59200|     2012649|    1569.3314| NA          |
| Liberia                  | Africa    |  1952|  38.48000|      863308|     575.5730| NA          |
| Liberia                  | Africa    |  1957|  39.48600|      975950|     620.9700| NA          |
| Liberia                  | Africa    |  1962|  40.50200|     1112796|     634.1952| NA          |
| Liberia                  | Africa    |  1967|  41.53600|     1279406|     713.6036| NA          |
| Liberia                  | Africa    |  1972|  42.61400|     1482628|     803.0055| NA          |
| Liberia                  | Africa    |  1977|  43.76400|     1703617|     640.3224| NA          |
| Liberia                  | Africa    |  1982|  44.85200|     1956875|     572.1996| NA          |
| Liberia                  | Africa    |  1987|  46.02700|     2269414|     506.1139| NA          |
| Liberia                  | Africa    |  1992|  40.80200|     1912974|     636.6229| NA          |
| Liberia                  | Africa    |  1997|  42.22100|     2200725|     609.1740| NA          |
| Liberia                  | Africa    |  2002|  43.75300|     2814651|     531.4824| NA          |
| Liberia                  | Africa    |  2007|  45.67800|     3193942|     414.5073| NA          |
| Libya                    | Africa    |  1952|  42.72300|     1019729|    2387.5481| NA          |
| Libya                    | Africa    |  1957|  45.28900|     1201578|    3448.2844| NA          |
| Libya                    | Africa    |  1962|  47.80800|     1441863|    6757.0308| NA          |
| Libya                    | Africa    |  1967|  50.22700|     1759224|   18772.7517| NA          |
| Libya                    | Africa    |  1972|  52.77300|     2183877|   21011.4972| NA          |
| Libya                    | Africa    |  1977|  57.44200|     2721783|   21951.2118| NA          |
| Libya                    | Africa    |  1982|  62.15500|     3344074|   17364.2754| NA          |
| Libya                    | Africa    |  1987|  66.23400|     3799845|   11770.5898| NA          |
| Libya                    | Africa    |  1992|  68.75500|     4364501|    9640.1385| NA          |
| Libya                    | Africa    |  1997|  71.55500|     4759670|    9467.4461| NA          |
| Libya                    | Africa    |  2002|  72.73700|     5368585|    9534.6775| NA          |
| Libya                    | Africa    |  2007|  73.95200|     6036914|   12057.4993| NA          |
| Madagascar               | Africa    |  1952|  36.68100|     4762912|    1443.0117| NA          |
| Madagascar               | Africa    |  1957|  38.86500|     5181679|    1589.2027| NA          |
| Madagascar               | Africa    |  1962|  40.84800|     5703324|    1643.3871| NA          |
| Madagascar               | Africa    |  1967|  42.88100|     6334556|    1634.0473| NA          |
| Madagascar               | Africa    |  1972|  44.85100|     7082430|    1748.5630| NA          |
| Madagascar               | Africa    |  1977|  46.88100|     8007166|    1544.2286| NA          |
| Madagascar               | Africa    |  1982|  48.96900|     9171477|    1302.8787| NA          |
| Madagascar               | Africa    |  1987|  49.35000|    10568642|    1155.4419| NA          |
| Madagascar               | Africa    |  1992|  52.21400|    12210395|    1040.6762| NA          |
| Madagascar               | Africa    |  1997|  54.97800|    14165114|     986.2959| NA          |
| Madagascar               | Africa    |  2002|  57.28600|    16473477|     894.6371| NA          |
| Madagascar               | Africa    |  2007|  59.44300|    19167654|    1044.7701| NA          |
| Malawi                   | Africa    |  1952|  36.25600|     2917802|     369.1651| NA          |
| Malawi                   | Africa    |  1957|  37.20700|     3221238|     416.3698| NA          |
| Malawi                   | Africa    |  1962|  38.41000|     3628608|     427.9011| NA          |
| Malawi                   | Africa    |  1967|  39.48700|     4147252|     495.5148| NA          |
| Malawi                   | Africa    |  1972|  41.76600|     4730997|     584.6220| NA          |
| Malawi                   | Africa    |  1977|  43.76700|     5637246|     663.2237| NA          |
| Malawi                   | Africa    |  1982|  45.64200|     6502825|     632.8039| NA          |
| Malawi                   | Africa    |  1987|  47.45700|     7824747|     635.5174| NA          |
| Malawi                   | Africa    |  1992|  49.42000|    10014249|     563.2000| NA          |
| Malawi                   | Africa    |  1997|  47.49500|    10419991|     692.2758| NA          |
| Malawi                   | Africa    |  2002|  45.00900|    11824495|     665.4231| NA          |
| Malawi                   | Africa    |  2007|  48.30300|    13327079|     759.3499| NA          |
| Malaysia                 | Asia      |  1952|  48.46300|     6748378|    1831.1329| NA          |
| Malaysia                 | Asia      |  1957|  52.10200|     7739235|    1810.0670| NA          |
| Malaysia                 | Asia      |  1962|  55.73700|     8906385|    2036.8849| NA          |
| Malaysia                 | Asia      |  1967|  59.37100|    10154878|    2277.7424| NA          |
| Malaysia                 | Asia      |  1972|  63.01000|    11441462|    2849.0948| NA          |
| Malaysia                 | Asia      |  1977|  65.25600|    12845381|    3827.9216| NA          |
| Malaysia                 | Asia      |  1982|  68.00000|    14441916|    4920.3560| NA          |
| Malaysia                 | Asia      |  1987|  69.50000|    16331785|    5249.8027| NA          |
| Malaysia                 | Asia      |  1992|  70.69300|    18319502|    7277.9128| NA          |
| Malaysia                 | Asia      |  1997|  71.93800|    20476091|   10132.9096| NA          |
| Malaysia                 | Asia      |  2002|  73.04400|    22662365|   10206.9779| NA          |
| Malaysia                 | Asia      |  2007|  74.24100|    24821286|   12451.6558| NA          |
| Mali                     | Africa    |  1952|  33.68500|     3838168|     452.3370| NA          |
| Mali                     | Africa    |  1957|  35.30700|     4241884|     490.3822| NA          |
| Mali                     | Africa    |  1962|  36.93600|     4690372|     496.1743| NA          |
| Mali                     | Africa    |  1967|  38.48700|     5212416|     545.0099| NA          |
| Mali                     | Africa    |  1972|  39.97700|     5828158|     581.3689| NA          |
| Mali                     | Africa    |  1977|  41.71400|     6491649|     686.3953| NA          |
| Mali                     | Africa    |  1982|  43.91600|     6998256|     618.0141| NA          |
| Mali                     | Africa    |  1987|  46.36400|     7634008|     684.1716| NA          |
| Mali                     | Africa    |  1992|  48.38800|     8416215|     739.0144| NA          |
| Mali                     | Africa    |  1997|  49.90300|     9384984|     790.2580| NA          |
| Mali                     | Africa    |  2002|  51.81800|    10580176|     951.4098| NA          |
| Mali                     | Africa    |  2007|  54.46700|    12031795|    1042.5816| NA          |
| Mauritania               | Africa    |  1952|  40.54300|     1022556|     743.1159| NA          |
| Mauritania               | Africa    |  1957|  42.33800|     1076852|     846.1203| NA          |
| Mauritania               | Africa    |  1962|  44.24800|     1146757|    1055.8960| NA          |
| Mauritania               | Africa    |  1967|  46.28900|     1230542|    1421.1452| NA          |
| Mauritania               | Africa    |  1972|  48.43700|     1332786|    1586.8518| NA          |
| Mauritania               | Africa    |  1977|  50.85200|     1456688|    1497.4922| NA          |
| Mauritania               | Africa    |  1982|  53.59900|     1622136|    1481.1502| NA          |
| Mauritania               | Africa    |  1987|  56.14500|     1841240|    1421.6036| NA          |
| Mauritania               | Africa    |  1992|  58.33300|     2119465|    1361.3698| NA          |
| Mauritania               | Africa    |  1997|  60.43000|     2444741|    1483.1361| NA          |
| Mauritania               | Africa    |  2002|  62.24700|     2828858|    1579.0195| NA          |
| Mauritania               | Africa    |  2007|  64.16400|     3270065|    1803.1515| NA          |
| Mauritius                | Africa    |  1952|  50.98600|      516556|    1967.9557| NA          |
| Mauritius                | Africa    |  1957|  58.08900|      609816|    2034.0380| NA          |
| Mauritius                | Africa    |  1962|  60.24600|      701016|    2529.0675| NA          |
| Mauritius                | Africa    |  1967|  61.55700|      789309|    2475.3876| NA          |
| Mauritius                | Africa    |  1972|  62.94400|      851334|    2575.4842| NA          |
| Mauritius                | Africa    |  1977|  64.93000|      913025|    3710.9830| NA          |
| Mauritius                | Africa    |  1982|  66.71100|      992040|    3688.0377| NA          |
| Mauritius                | Africa    |  1987|  68.74000|     1042663|    4783.5869| NA          |
| Mauritius                | Africa    |  1992|  69.74500|     1096202|    6058.2538| NA          |
| Mauritius                | Africa    |  1997|  70.73600|     1149818|    7425.7053| NA          |
| Mauritius                | Africa    |  2002|  71.95400|     1200206|    9021.8159| NA          |
| Mauritius                | Africa    |  2007|  72.80100|     1250882|   10956.9911| NA          |
| Mexico                   | Americas  |  1952|  50.78900|    30144317|    3478.1255| Mexico City |
| Mexico                   | Americas  |  1957|  55.19000|    35015548|    4131.5466| Mexico City |
| Mexico                   | Americas  |  1962|  58.29900|    41121485|    4581.6094| Mexico City |
| Mexico                   | Americas  |  1967|  60.11000|    47995559|    5754.7339| Mexico City |
| Mexico                   | Americas  |  1972|  62.36100|    55984294|    6809.4067| Mexico City |
| Mexico                   | Americas  |  1977|  65.03200|    63759976|    7674.9291| Mexico City |
| Mexico                   | Americas  |  1982|  67.40500|    71640904|    9611.1475| Mexico City |
| Mexico                   | Americas  |  1987|  69.49800|    80122492|    8688.1560| Mexico City |
| Mexico                   | Americas  |  1992|  71.45500|    88111030|    9472.3843| Mexico City |
| Mexico                   | Americas  |  1997|  73.67000|    95895146|    9767.2975| Mexico City |
| Mexico                   | Americas  |  2002|  74.90200|   102479927|   10742.4405| Mexico City |
| Mexico                   | Americas  |  2007|  76.19500|   108700891|   11977.5750| Mexico City |
| Mongolia                 | Asia      |  1952|  42.24400|      800663|     786.5669| NA          |
| Mongolia                 | Asia      |  1957|  45.24800|      882134|     912.6626| NA          |
| Mongolia                 | Asia      |  1962|  48.25100|     1010280|    1056.3540| NA          |
| Mongolia                 | Asia      |  1967|  51.25300|     1149500|    1226.0411| NA          |
| Mongolia                 | Asia      |  1972|  53.75400|     1320500|    1421.7420| NA          |
| Mongolia                 | Asia      |  1977|  55.49100|     1528000|    1647.5117| NA          |
| Mongolia                 | Asia      |  1982|  57.48900|     1756032|    2000.6031| NA          |
| Mongolia                 | Asia      |  1987|  60.22200|     2015133|    2338.0083| NA          |
| Mongolia                 | Asia      |  1992|  61.27100|     2312802|    1785.4020| NA          |
| Mongolia                 | Asia      |  1997|  63.62500|     2494803|    1902.2521| NA          |
| Mongolia                 | Asia      |  2002|  65.03300|     2674234|    2140.7393| NA          |
| Mongolia                 | Asia      |  2007|  66.80300|     2874127|    3095.7723| NA          |
| Montenegro               | Europe    |  1952|  59.16400|      413834|    2647.5856| NA          |
| Montenegro               | Europe    |  1957|  61.44800|      442829|    3682.2599| NA          |
| Montenegro               | Europe    |  1962|  63.72800|      474528|    4649.5938| NA          |
| Montenegro               | Europe    |  1967|  67.17800|      501035|    5907.8509| NA          |
| Montenegro               | Europe    |  1972|  70.63600|      527678|    7778.4140| NA          |
| Montenegro               | Europe    |  1977|  73.06600|      560073|    9595.9299| NA          |
| Montenegro               | Europe    |  1982|  74.10100|      562548|   11222.5876| NA          |
| Montenegro               | Europe    |  1987|  74.86500|      569473|   11732.5102| NA          |
| Montenegro               | Europe    |  1992|  75.43500|      621621|    7003.3390| NA          |
| Montenegro               | Europe    |  1997|  75.44500|      692651|    6465.6133| NA          |
| Montenegro               | Europe    |  2002|  73.98100|      720230|    6557.1943| NA          |
| Montenegro               | Europe    |  2007|  74.54300|      684736|    9253.8961| NA          |
| Morocco                  | Africa    |  1952|  42.87300|     9939217|    1688.2036| NA          |
| Morocco                  | Africa    |  1957|  45.42300|    11406350|    1642.0023| NA          |
| Morocco                  | Africa    |  1962|  47.92400|    13056604|    1566.3535| NA          |
| Morocco                  | Africa    |  1967|  50.33500|    14770296|    1711.0448| NA          |
| Morocco                  | Africa    |  1972|  52.86200|    16660670|    1930.1950| NA          |
| Morocco                  | Africa    |  1977|  55.73000|    18396941|    2370.6200| NA          |
| Morocco                  | Africa    |  1982|  59.65000|    20198730|    2702.6204| NA          |
| Morocco                  | Africa    |  1987|  62.67700|    22987397|    2755.0470| NA          |
| Morocco                  | Africa    |  1992|  65.39300|    25798239|    2948.0473| NA          |
| Morocco                  | Africa    |  1997|  67.66000|    28529501|    2982.1019| NA          |
| Morocco                  | Africa    |  2002|  69.61500|    31167783|    3258.4956| NA          |
| Morocco                  | Africa    |  2007|  71.16400|    33757175|    3820.1752| NA          |
| Mozambique               | Africa    |  1952|  31.28600|     6446316|     468.5260| NA          |
| Mozambique               | Africa    |  1957|  33.77900|     7038035|     495.5868| NA          |
| Mozambique               | Africa    |  1962|  36.16100|     7788944|     556.6864| NA          |
| Mozambique               | Africa    |  1967|  38.11300|     8680909|     566.6692| NA          |
| Mozambique               | Africa    |  1972|  40.32800|     9809596|     724.9178| NA          |
| Mozambique               | Africa    |  1977|  42.49500|    11127868|     502.3197| NA          |
| Mozambique               | Africa    |  1982|  42.79500|    12587223|     462.2114| NA          |
| Mozambique               | Africa    |  1987|  42.86100|    12891952|     389.8762| NA          |
| Mozambique               | Africa    |  1992|  44.28400|    13160731|     410.8968| NA          |
| Mozambique               | Africa    |  1997|  46.34400|    16603334|     472.3461| NA          |
| Mozambique               | Africa    |  2002|  44.02600|    18473780|     633.6179| NA          |
| Mozambique               | Africa    |  2007|  42.08200|    19951656|     823.6856| NA          |
| Myanmar                  | Asia      |  1952|  36.31900|    20092996|     331.0000| NA          |
| Myanmar                  | Asia      |  1957|  41.90500|    21731844|     350.0000| NA          |
| Myanmar                  | Asia      |  1962|  45.10800|    23634436|     388.0000| NA          |
| Myanmar                  | Asia      |  1967|  49.37900|    25870271|     349.0000| NA          |
| Myanmar                  | Asia      |  1972|  53.07000|    28466390|     357.0000| NA          |
| Myanmar                  | Asia      |  1977|  56.05900|    31528087|     371.0000| NA          |
| Myanmar                  | Asia      |  1982|  58.05600|    34680442|     424.0000| NA          |
| Myanmar                  | Asia      |  1987|  58.33900|    38028578|     385.0000| NA          |
| Myanmar                  | Asia      |  1992|  59.32000|    40546538|     347.0000| NA          |
| Myanmar                  | Asia      |  1997|  60.32800|    43247867|     415.0000| NA          |
| Myanmar                  | Asia      |  2002|  59.90800|    45598081|     611.0000| NA          |
| Myanmar                  | Asia      |  2007|  62.06900|    47761980|     944.0000| NA          |
| Namibia                  | Africa    |  1952|  41.72500|      485831|    2423.7804| NA          |
| Namibia                  | Africa    |  1957|  45.22600|      548080|    2621.4481| NA          |
| Namibia                  | Africa    |  1962|  48.38600|      621392|    3173.2156| NA          |
| Namibia                  | Africa    |  1967|  51.15900|      706640|    3793.6948| NA          |
| Namibia                  | Africa    |  1972|  53.86700|      821782|    3746.0809| NA          |
| Namibia                  | Africa    |  1977|  56.43700|      977026|    3876.4860| NA          |
| Namibia                  | Africa    |  1982|  58.96800|     1099010|    4191.1005| NA          |
| Namibia                  | Africa    |  1987|  60.83500|     1278184|    3693.7313| NA          |
| Namibia                  | Africa    |  1992|  61.99900|     1554253|    3804.5380| NA          |
| Namibia                  | Africa    |  1997|  58.90900|     1774766|    3899.5243| NA          |
| Namibia                  | Africa    |  2002|  51.47900|     1972153|    4072.3248| NA          |
| Namibia                  | Africa    |  2007|  52.90600|     2055080|    4811.0604| NA          |
| Nepal                    | Asia      |  1952|  36.15700|     9182536|     545.8657| NA          |
| Nepal                    | Asia      |  1957|  37.68600|     9682338|     597.9364| NA          |
| Nepal                    | Asia      |  1962|  39.39300|    10332057|     652.3969| NA          |
| Nepal                    | Asia      |  1967|  41.47200|    11261690|     676.4422| NA          |
| Nepal                    | Asia      |  1972|  43.97100|    12412593|     674.7881| NA          |
| Nepal                    | Asia      |  1977|  46.74800|    13933198|     694.1124| NA          |
| Nepal                    | Asia      |  1982|  49.59400|    15796314|     718.3731| NA          |
| Nepal                    | Asia      |  1987|  52.53700|    17917180|     775.6325| NA          |
| Nepal                    | Asia      |  1992|  55.72700|    20326209|     897.7404| NA          |
| Nepal                    | Asia      |  1997|  59.42600|    23001113|    1010.8921| NA          |
| Nepal                    | Asia      |  2002|  61.34000|    25873917|    1057.2063| NA          |
| Nepal                    | Asia      |  2007|  63.78500|    28901790|    1091.3598| NA          |
| Netherlands              | Europe    |  1952|  72.13000|    10381988|    8941.5719| NA          |
| Netherlands              | Europe    |  1957|  72.99000|    11026383|   11276.1934| NA          |
| Netherlands              | Europe    |  1962|  73.23000|    11805689|   12790.8496| NA          |
| Netherlands              | Europe    |  1967|  73.82000|    12596822|   15363.2514| NA          |
| Netherlands              | Europe    |  1972|  73.75000|    13329874|   18794.7457| NA          |
| Netherlands              | Europe    |  1977|  75.24000|    13852989|   21209.0592| NA          |
| Netherlands              | Europe    |  1982|  76.05000|    14310401|   21399.4605| NA          |
| Netherlands              | Europe    |  1987|  76.83000|    14665278|   23651.3236| NA          |
| Netherlands              | Europe    |  1992|  77.42000|    15174244|   26790.9496| NA          |
| Netherlands              | Europe    |  1997|  78.03000|    15604464|   30246.1306| NA          |
| Netherlands              | Europe    |  2002|  78.53000|    16122830|   33724.7578| NA          |
| Netherlands              | Europe    |  2007|  79.76200|    16570613|   36797.9333| NA          |
| New Zealand              | Oceania   |  1952|  69.39000|     1994794|   10556.5757| Wellington  |
| New Zealand              | Oceania   |  1957|  70.26000|     2229407|   12247.3953| Wellington  |
| New Zealand              | Oceania   |  1962|  71.24000|     2488550|   13175.6780| Wellington  |
| New Zealand              | Oceania   |  1967|  71.52000|     2728150|   14463.9189| Wellington  |
| New Zealand              | Oceania   |  1972|  71.89000|     2929100|   16046.0373| Wellington  |
| New Zealand              | Oceania   |  1977|  72.22000|     3164900|   16233.7177| Wellington  |
| New Zealand              | Oceania   |  1982|  73.84000|     3210650|   17632.4104| Wellington  |
| New Zealand              | Oceania   |  1987|  74.32000|     3317166|   19007.1913| Wellington  |
| New Zealand              | Oceania   |  1992|  76.33000|     3437674|   18363.3249| Wellington  |
| New Zealand              | Oceania   |  1997|  77.55000|     3676187|   21050.4138| Wellington  |
| New Zealand              | Oceania   |  2002|  79.11000|     3908037|   23189.8014| Wellington  |
| New Zealand              | Oceania   |  2007|  80.20400|     4115771|   25185.0091| Wellington  |
| Nicaragua                | Americas  |  1952|  42.31400|     1165790|    3112.3639| NA          |
| Nicaragua                | Americas  |  1957|  45.43200|     1358828|    3457.4159| NA          |
| Nicaragua                | Americas  |  1962|  48.63200|     1590597|    3634.3644| NA          |
| Nicaragua                | Americas  |  1967|  51.88400|     1865490|    4643.3935| NA          |
| Nicaragua                | Americas  |  1972|  55.15100|     2182908|    4688.5933| NA          |
| Nicaragua                | Americas  |  1977|  57.47000|     2554598|    5486.3711| NA          |
| Nicaragua                | Americas  |  1982|  59.29800|     2979423|    3470.3382| NA          |
| Nicaragua                | Americas  |  1987|  62.00800|     3344353|    2955.9844| NA          |
| Nicaragua                | Americas  |  1992|  65.84300|     4017939|    2170.1517| NA          |
| Nicaragua                | Americas  |  1997|  68.42600|     4609572|    2253.0230| NA          |
| Nicaragua                | Americas  |  2002|  70.83600|     5146848|    2474.5488| NA          |
| Nicaragua                | Americas  |  2007|  72.89900|     5675356|    2749.3210| NA          |
| Niger                    | Africa    |  1952|  37.44400|     3379468|     761.8794| NA          |
| Niger                    | Africa    |  1957|  38.59800|     3692184|     835.5234| NA          |
| Niger                    | Africa    |  1962|  39.48700|     4076008|     997.7661| NA          |
| Niger                    | Africa    |  1967|  40.11800|     4534062|    1054.3849| NA          |
| Niger                    | Africa    |  1972|  40.54600|     5060262|     954.2092| NA          |
| Niger                    | Africa    |  1977|  41.29100|     5682086|     808.8971| NA          |
| Niger                    | Africa    |  1982|  42.59800|     6437188|     909.7221| NA          |
| Niger                    | Africa    |  1987|  44.55500|     7332638|     668.3000| NA          |
| Niger                    | Africa    |  1992|  47.39100|     8392818|     581.1827| NA          |
| Niger                    | Africa    |  1997|  51.31300|     9666252|     580.3052| NA          |
| Niger                    | Africa    |  2002|  54.49600|    11140655|     601.0745| NA          |
| Niger                    | Africa    |  2007|  56.86700|    12894865|     619.6769| NA          |
| Nigeria                  | Africa    |  1952|  36.32400|    33119096|    1077.2819| NA          |
| Nigeria                  | Africa    |  1957|  37.80200|    37173340|    1100.5926| NA          |
| Nigeria                  | Africa    |  1962|  39.36000|    41871351|    1150.9275| NA          |
| Nigeria                  | Africa    |  1967|  41.04000|    47287752|    1014.5141| NA          |
| Nigeria                  | Africa    |  1972|  42.82100|    53740085|    1698.3888| NA          |
| Nigeria                  | Africa    |  1977|  44.51400|    62209173|    1981.9518| NA          |
| Nigeria                  | Africa    |  1982|  45.82600|    73039376|    1576.9738| NA          |
| Nigeria                  | Africa    |  1987|  46.88600|    81551520|    1385.0296| NA          |
| Nigeria                  | Africa    |  1992|  47.47200|    93364244|    1619.8482| NA          |
| Nigeria                  | Africa    |  1997|  47.46400|   106207839|    1624.9413| NA          |
| Nigeria                  | Africa    |  2002|  46.60800|   119901274|    1615.2864| NA          |
| Nigeria                  | Africa    |  2007|  46.85900|   135031164|    2013.9773| NA          |
| Norway                   | Europe    |  1952|  72.67000|     3327728|   10095.4217| Oslo        |
| Norway                   | Europe    |  1957|  73.44000|     3491938|   11653.9730| Oslo        |
| Norway                   | Europe    |  1962|  73.47000|     3638919|   13450.4015| Oslo        |
| Norway                   | Europe    |  1967|  74.08000|     3786019|   16361.8765| Oslo        |
| Norway                   | Europe    |  1972|  74.34000|     3933004|   18965.0555| Oslo        |
| Norway                   | Europe    |  1977|  75.37000|     4043205|   23311.3494| Oslo        |
| Norway                   | Europe    |  1982|  75.97000|     4114787|   26298.6353| Oslo        |
| Norway                   | Europe    |  1987|  75.89000|     4186147|   31540.9748| Oslo        |
| Norway                   | Europe    |  1992|  77.32000|     4286357|   33965.6611| Oslo        |
| Norway                   | Europe    |  1997|  78.32000|     4405672|   41283.1643| Oslo        |
| Norway                   | Europe    |  2002|  79.05000|     4535591|   44683.9753| Oslo        |
| Norway                   | Europe    |  2007|  80.19600|     4627926|   49357.1902| Oslo        |
| Oman                     | Asia      |  1952|  37.57800|      507833|    1828.2303| NA          |
| Oman                     | Asia      |  1957|  40.08000|      561977|    2242.7466| NA          |
| Oman                     | Asia      |  1962|  43.16500|      628164|    2924.6381| NA          |
| Oman                     | Asia      |  1967|  46.98800|      714775|    4720.9427| NA          |
| Oman                     | Asia      |  1972|  52.14300|      829050|   10618.0385| NA          |
| Oman                     | Asia      |  1977|  57.36700|     1004533|   11848.3439| NA          |
| Oman                     | Asia      |  1982|  62.72800|     1301048|   12954.7910| NA          |
| Oman                     | Asia      |  1987|  67.73400|     1593882|   18115.2231| NA          |
| Oman                     | Asia      |  1992|  71.19700|     1915208|   18616.7069| NA          |
| Oman                     | Asia      |  1997|  72.49900|     2283635|   19702.0558| NA          |
| Oman                     | Asia      |  2002|  74.19300|     2713462|   19774.8369| NA          |
| Oman                     | Asia      |  2007|  75.64000|     3204897|   22316.1929| NA          |
| Pakistan                 | Asia      |  1952|  43.43600|    41346560|     684.5971| NA          |
| Pakistan                 | Asia      |  1957|  45.55700|    46679944|     747.0835| NA          |
| Pakistan                 | Asia      |  1962|  47.67000|    53100671|     803.3427| NA          |
| Pakistan                 | Asia      |  1967|  49.80000|    60641899|     942.4083| NA          |
| Pakistan                 | Asia      |  1972|  51.92900|    69325921|    1049.9390| NA          |
| Pakistan                 | Asia      |  1977|  54.04300|    78152686|    1175.9212| NA          |
| Pakistan                 | Asia      |  1982|  56.15800|    91462088|    1443.4298| NA          |
| Pakistan                 | Asia      |  1987|  58.24500|   105186881|    1704.6866| NA          |
| Pakistan                 | Asia      |  1992|  60.83800|   120065004|    1971.8295| NA          |
| Pakistan                 | Asia      |  1997|  61.81800|   135564834|    2049.3505| NA          |
| Pakistan                 | Asia      |  2002|  63.61000|   153403524|    2092.7124| NA          |
| Pakistan                 | Asia      |  2007|  65.48300|   169270617|    2605.9476| NA          |
| Panama                   | Americas  |  1952|  55.19100|      940080|    2480.3803| NA          |
| Panama                   | Americas  |  1957|  59.20100|     1063506|    2961.8009| NA          |
| Panama                   | Americas  |  1962|  61.81700|     1215725|    3536.5403| NA          |
| Panama                   | Americas  |  1967|  64.07100|     1405486|    4421.0091| NA          |
| Panama                   | Americas  |  1972|  66.21600|     1616384|    5364.2497| NA          |
| Panama                   | Americas  |  1977|  68.68100|     1839782|    5351.9121| NA          |
| Panama                   | Americas  |  1982|  70.47200|     2036305|    7009.6016| NA          |
| Panama                   | Americas  |  1987|  71.52300|     2253639|    7034.7792| NA          |
| Panama                   | Americas  |  1992|  72.46200|     2484997|    6618.7431| NA          |
| Panama                   | Americas  |  1997|  73.73800|     2734531|    7113.6923| NA          |
| Panama                   | Americas  |  2002|  74.71200|     2990875|    7356.0319| NA          |
| Panama                   | Americas  |  2007|  75.53700|     3242173|    9809.1856| NA          |
| Paraguay                 | Americas  |  1952|  62.64900|     1555876|    1952.3087| NA          |
| Paraguay                 | Americas  |  1957|  63.19600|     1770902|    2046.1547| NA          |
| Paraguay                 | Americas  |  1962|  64.36100|     2009813|    2148.0271| NA          |
| Paraguay                 | Americas  |  1967|  64.95100|     2287985|    2299.3763| NA          |
| Paraguay                 | Americas  |  1972|  65.81500|     2614104|    2523.3380| NA          |
| Paraguay                 | Americas  |  1977|  66.35300|     2984494|    3248.3733| NA          |
| Paraguay                 | Americas  |  1982|  66.87400|     3366439|    4258.5036| NA          |
| Paraguay                 | Americas  |  1987|  67.37800|     3886512|    3998.8757| NA          |
| Paraguay                 | Americas  |  1992|  68.22500|     4483945|    4196.4111| NA          |
| Paraguay                 | Americas  |  1997|  69.40000|     5154123|    4247.4003| NA          |
| Paraguay                 | Americas  |  2002|  70.75500|     5884491|    3783.6742| NA          |
| Paraguay                 | Americas  |  2007|  71.75200|     6667147|    4172.8385| NA          |
| Peru                     | Americas  |  1952|  43.90200|     8025700|    3758.5234| NA          |
| Peru                     | Americas  |  1957|  46.26300|     9146100|    4245.2567| NA          |
| Peru                     | Americas  |  1962|  49.09600|    10516500|    4957.0380| NA          |
| Peru                     | Americas  |  1967|  51.44500|    12132200|    5788.0933| NA          |
| Peru                     | Americas  |  1972|  55.44800|    13954700|    5937.8273| NA          |
| Peru                     | Americas  |  1977|  58.44700|    15990099|    6281.2909| NA          |
| Peru                     | Americas  |  1982|  61.40600|    18125129|    6434.5018| NA          |
| Peru                     | Americas  |  1987|  64.13400|    20195924|    6360.9434| NA          |
| Peru                     | Americas  |  1992|  66.45800|    22430449|    4446.3809| NA          |
| Peru                     | Americas  |  1997|  68.38600|    24748122|    5838.3477| NA          |
| Peru                     | Americas  |  2002|  69.90600|    26769436|    5909.0201| NA          |
| Peru                     | Americas  |  2007|  71.42100|    28674757|    7408.9056| NA          |
| Philippines              | Asia      |  1952|  47.75200|    22438691|    1272.8810| NA          |
| Philippines              | Asia      |  1957|  51.33400|    26072194|    1547.9448| NA          |
| Philippines              | Asia      |  1962|  54.75700|    30325264|    1649.5522| NA          |
| Philippines              | Asia      |  1967|  56.39300|    35356600|    1814.1274| NA          |
| Philippines              | Asia      |  1972|  58.06500|    40850141|    1989.3741| NA          |
| Philippines              | Asia      |  1977|  60.06000|    46850962|    2373.2043| NA          |
| Philippines              | Asia      |  1982|  62.08200|    53456774|    2603.2738| NA          |
| Philippines              | Asia      |  1987|  64.15100|    60017788|    2189.6350| NA          |
| Philippines              | Asia      |  1992|  66.45800|    67185766|    2279.3240| NA          |
| Philippines              | Asia      |  1997|  68.56400|    75012988|    2536.5349| NA          |
| Philippines              | Asia      |  2002|  70.30300|    82995088|    2650.9211| NA          |
| Philippines              | Asia      |  2007|  71.68800|    91077287|    3190.4810| NA          |
| Poland                   | Europe    |  1952|  61.31000|    25730551|    4029.3297| NA          |
| Poland                   | Europe    |  1957|  65.77000|    28235346|    4734.2530| NA          |
| Poland                   | Europe    |  1962|  67.64000|    30329617|    5338.7521| NA          |
| Poland                   | Europe    |  1967|  69.61000|    31785378|    6557.1528| NA          |
| Poland                   | Europe    |  1972|  70.85000|    33039545|    8006.5070| NA          |
| Poland                   | Europe    |  1977|  70.67000|    34621254|    9508.1415| NA          |
| Poland                   | Europe    |  1982|  71.32000|    36227381|    8451.5310| NA          |
| Poland                   | Europe    |  1987|  70.98000|    37740710|    9082.3512| NA          |
| Poland                   | Europe    |  1992|  70.99000|    38370697|    7738.8812| NA          |
| Poland                   | Europe    |  1997|  72.75000|    38654957|   10159.5837| NA          |
| Poland                   | Europe    |  2002|  74.67000|    38625976|   12002.2391| NA          |
| Poland                   | Europe    |  2007|  75.56300|    38518241|   15389.9247| NA          |
| Portugal                 | Europe    |  1952|  59.82000|     8526050|    3068.3199| NA          |
| Portugal                 | Europe    |  1957|  61.51000|     8817650|    3774.5717| NA          |
| Portugal                 | Europe    |  1962|  64.39000|     9019800|    4727.9549| NA          |
| Portugal                 | Europe    |  1967|  66.60000|     9103000|    6361.5180| NA          |
| Portugal                 | Europe    |  1972|  69.26000|     8970450|    9022.2474| NA          |
| Portugal                 | Europe    |  1977|  70.41000|     9662600|   10172.4857| NA          |
| Portugal                 | Europe    |  1982|  72.77000|     9859650|   11753.8429| NA          |
| Portugal                 | Europe    |  1987|  74.06000|     9915289|   13039.3088| NA          |
| Portugal                 | Europe    |  1992|  74.86000|     9927680|   16207.2666| NA          |
| Portugal                 | Europe    |  1997|  75.97000|    10156415|   17641.0316| NA          |
| Portugal                 | Europe    |  2002|  77.29000|    10433867|   19970.9079| NA          |
| Portugal                 | Europe    |  2007|  78.09800|    10642836|   20509.6478| NA          |
| Puerto Rico              | Americas  |  1952|  64.28000|     2227000|    3081.9598| NA          |
| Puerto Rico              | Americas  |  1957|  68.54000|     2260000|    3907.1562| NA          |
| Puerto Rico              | Americas  |  1962|  69.62000|     2448046|    5108.3446| NA          |
| Puerto Rico              | Americas  |  1967|  71.10000|     2648961|    6929.2777| NA          |
| Puerto Rico              | Americas  |  1972|  72.16000|     2847132|    9123.0417| NA          |
| Puerto Rico              | Americas  |  1977|  73.44000|     3080828|    9770.5249| NA          |
| Puerto Rico              | Americas  |  1982|  73.75000|     3279001|   10330.9891| NA          |
| Puerto Rico              | Americas  |  1987|  74.63000|     3444468|   12281.3419| NA          |
| Puerto Rico              | Americas  |  1992|  73.91100|     3585176|   14641.5871| NA          |
| Puerto Rico              | Americas  |  1997|  74.91700|     3759430|   16999.4333| NA          |
| Puerto Rico              | Americas  |  2002|  77.77800|     3859606|   18855.6062| NA          |
| Puerto Rico              | Americas  |  2007|  78.74600|     3942491|   19328.7090| NA          |
| Reunion                  | Africa    |  1952|  52.72400|      257700|    2718.8853| NA          |
| Reunion                  | Africa    |  1957|  55.09000|      308700|    2769.4518| NA          |
| Reunion                  | Africa    |  1962|  57.66600|      358900|    3173.7233| NA          |
| Reunion                  | Africa    |  1967|  60.54200|      414024|    4021.1757| NA          |
| Reunion                  | Africa    |  1972|  64.27400|      461633|    5047.6586| NA          |
| Reunion                  | Africa    |  1977|  67.06400|      492095|    4319.8041| NA          |
| Reunion                  | Africa    |  1982|  69.88500|      517810|    5267.2194| NA          |
| Reunion                  | Africa    |  1987|  71.91300|      562035|    5303.3775| NA          |
| Reunion                  | Africa    |  1992|  73.61500|      622191|    6101.2558| NA          |
| Reunion                  | Africa    |  1997|  74.77200|      684810|    6071.9414| NA          |
| Reunion                  | Africa    |  2002|  75.74400|      743981|    6316.1652| NA          |
| Reunion                  | Africa    |  2007|  76.44200|      798094|    7670.1226| NA          |
| Romania                  | Europe    |  1952|  61.05000|    16630000|    3144.6132| NA          |
| Romania                  | Europe    |  1957|  64.10000|    17829327|    3943.3702| NA          |
| Romania                  | Europe    |  1962|  66.80000|    18680721|    4734.9976| NA          |
| Romania                  | Europe    |  1967|  66.80000|    19284814|    6470.8665| NA          |
| Romania                  | Europe    |  1972|  69.21000|    20662648|    8011.4144| NA          |
| Romania                  | Europe    |  1977|  69.46000|    21658597|    9356.3972| NA          |
| Romania                  | Europe    |  1982|  69.66000|    22356726|    9605.3141| NA          |
| Romania                  | Europe    |  1987|  69.53000|    22686371|    9696.2733| NA          |
| Romania                  | Europe    |  1992|  69.36000|    22797027|    6598.4099| NA          |
| Romania                  | Europe    |  1997|  69.72000|    22562458|    7346.5476| NA          |
| Romania                  | Europe    |  2002|  71.32200|    22404337|    7885.3601| NA          |
| Romania                  | Europe    |  2007|  72.47600|    22276056|   10808.4756| NA          |
| Rwanda                   | Africa    |  1952|  40.00000|     2534927|     493.3239| NA          |
| Rwanda                   | Africa    |  1957|  41.50000|     2822082|     540.2894| NA          |
| Rwanda                   | Africa    |  1962|  43.00000|     3051242|     597.4731| NA          |
| Rwanda                   | Africa    |  1967|  44.10000|     3451079|     510.9637| NA          |
| Rwanda                   | Africa    |  1972|  44.60000|     3992121|     590.5807| NA          |
| Rwanda                   | Africa    |  1977|  45.00000|     4657072|     670.0806| NA          |
| Rwanda                   | Africa    |  1982|  46.21800|     5507565|     881.5706| NA          |
| Rwanda                   | Africa    |  1987|  44.02000|     6349365|     847.9912| NA          |
| Rwanda                   | Africa    |  1992|  23.59900|     7290203|     737.0686| NA          |
| Rwanda                   | Africa    |  1997|  36.08700|     7212583|     589.9445| NA          |
| Rwanda                   | Africa    |  2002|  43.41300|     7852401|     785.6538| NA          |
| Rwanda                   | Africa    |  2007|  46.24200|     8860588|     863.0885| NA          |
| Sao Tome and Principe    | Africa    |  1952|  46.47100|       60011|     879.5836| NA          |
| Sao Tome and Principe    | Africa    |  1957|  48.94500|       61325|     860.7369| NA          |
| Sao Tome and Principe    | Africa    |  1962|  51.89300|       65345|    1071.5511| NA          |
| Sao Tome and Principe    | Africa    |  1967|  54.42500|       70787|    1384.8406| NA          |
| Sao Tome and Principe    | Africa    |  1972|  56.48000|       76595|    1532.9853| NA          |
| Sao Tome and Principe    | Africa    |  1977|  58.55000|       86796|    1737.5617| NA          |
| Sao Tome and Principe    | Africa    |  1982|  60.35100|       98593|    1890.2181| NA          |
| Sao Tome and Principe    | Africa    |  1987|  61.72800|      110812|    1516.5255| NA          |
| Sao Tome and Principe    | Africa    |  1992|  62.74200|      125911|    1428.7778| NA          |
| Sao Tome and Principe    | Africa    |  1997|  63.30600|      145608|    1339.0760| NA          |
| Sao Tome and Principe    | Africa    |  2002|  64.33700|      170372|    1353.0924| NA          |
| Sao Tome and Principe    | Africa    |  2007|  65.52800|      199579|    1598.4351| NA          |
| Saudi Arabia             | Asia      |  1952|  39.87500|     4005677|    6459.5548| NA          |
| Saudi Arabia             | Asia      |  1957|  42.86800|     4419650|    8157.5912| NA          |
| Saudi Arabia             | Asia      |  1962|  45.91400|     4943029|   11626.4197| NA          |
| Saudi Arabia             | Asia      |  1967|  49.90100|     5618198|   16903.0489| NA          |
| Saudi Arabia             | Asia      |  1972|  53.88600|     6472756|   24837.4287| NA          |
| Saudi Arabia             | Asia      |  1977|  58.69000|     8128505|   34167.7626| NA          |
| Saudi Arabia             | Asia      |  1982|  63.01200|    11254672|   33693.1753| NA          |
| Saudi Arabia             | Asia      |  1987|  66.29500|    14619745|   21198.2614| NA          |
| Saudi Arabia             | Asia      |  1992|  68.76800|    16945857|   24841.6178| NA          |
| Saudi Arabia             | Asia      |  1997|  70.53300|    21229759|   20586.6902| NA          |
| Saudi Arabia             | Asia      |  2002|  71.62600|    24501530|   19014.5412| NA          |
| Saudi Arabia             | Asia      |  2007|  72.77700|    27601038|   21654.8319| NA          |
| Senegal                  | Africa    |  1952|  37.27800|     2755589|    1450.3570| NA          |
| Senegal                  | Africa    |  1957|  39.32900|     3054547|    1567.6530| NA          |
| Senegal                  | Africa    |  1962|  41.45400|     3430243|    1654.9887| NA          |
| Senegal                  | Africa    |  1967|  43.56300|     3965841|    1612.4046| NA          |
| Senegal                  | Africa    |  1972|  45.81500|     4588696|    1597.7121| NA          |
| Senegal                  | Africa    |  1977|  48.87900|     5260855|    1561.7691| NA          |
| Senegal                  | Africa    |  1982|  52.37900|     6147783|    1518.4800| NA          |
| Senegal                  | Africa    |  1987|  55.76900|     7171347|    1441.7207| NA          |
| Senegal                  | Africa    |  1992|  58.19600|     8307920|    1367.8994| NA          |
| Senegal                  | Africa    |  1997|  60.18700|     9535314|    1392.3683| NA          |
| Senegal                  | Africa    |  2002|  61.60000|    10870037|    1519.6353| NA          |
| Senegal                  | Africa    |  2007|  63.06200|    12267493|    1712.4721| NA          |
| Serbia                   | Europe    |  1952|  57.99600|     6860147|    3581.4594| NA          |
| Serbia                   | Europe    |  1957|  61.68500|     7271135|    4981.0909| NA          |
| Serbia                   | Europe    |  1962|  64.53100|     7616060|    6289.6292| NA          |
| Serbia                   | Europe    |  1967|  66.91400|     7971222|    7991.7071| NA          |
| Serbia                   | Europe    |  1972|  68.70000|     8313288|   10522.0675| NA          |
| Serbia                   | Europe    |  1977|  70.30000|     8686367|   12980.6696| NA          |
| Serbia                   | Europe    |  1982|  70.16200|     9032824|   15181.0927| NA          |
| Serbia                   | Europe    |  1987|  71.21800|     9230783|   15870.8785| NA          |
| Serbia                   | Europe    |  1992|  71.65900|     9826397|    9325.0682| NA          |
| Serbia                   | Europe    |  1997|  72.23200|    10336594|    7914.3203| NA          |
| Serbia                   | Europe    |  2002|  73.21300|    10111559|    7236.0753| NA          |
| Serbia                   | Europe    |  2007|  74.00200|    10150265|    9786.5347| NA          |
| Sierra Leone             | Africa    |  1952|  30.33100|     2143249|     879.7877| NA          |
| Sierra Leone             | Africa    |  1957|  31.57000|     2295678|    1004.4844| NA          |
| Sierra Leone             | Africa    |  1962|  32.76700|     2467895|    1116.6399| NA          |
| Sierra Leone             | Africa    |  1967|  34.11300|     2662190|    1206.0435| NA          |
| Sierra Leone             | Africa    |  1972|  35.40000|     2879013|    1353.7598| NA          |
| Sierra Leone             | Africa    |  1977|  36.78800|     3140897|    1348.2852| NA          |
| Sierra Leone             | Africa    |  1982|  38.44500|     3464522|    1465.0108| NA          |
| Sierra Leone             | Africa    |  1987|  40.00600|     3868905|    1294.4478| NA          |
| Sierra Leone             | Africa    |  1992|  38.33300|     4260884|    1068.6963| NA          |
| Sierra Leone             | Africa    |  1997|  39.89700|     4578212|     574.6482| NA          |
| Sierra Leone             | Africa    |  2002|  41.01200|     5359092|     699.4897| NA          |
| Sierra Leone             | Africa    |  2007|  42.56800|     6144562|     862.5408| NA          |
| Singapore                | Asia      |  1952|  60.39600|     1127000|    2315.1382| NA          |
| Singapore                | Asia      |  1957|  63.17900|     1445929|    2843.1044| NA          |
| Singapore                | Asia      |  1962|  65.79800|     1750200|    3674.7356| NA          |
| Singapore                | Asia      |  1967|  67.94600|     1977600|    4977.4185| NA          |
| Singapore                | Asia      |  1972|  69.52100|     2152400|    8597.7562| NA          |
| Singapore                | Asia      |  1977|  70.79500|     2325300|   11210.0895| NA          |
| Singapore                | Asia      |  1982|  71.76000|     2651869|   15169.1611| NA          |
| Singapore                | Asia      |  1987|  73.56000|     2794552|   18861.5308| NA          |
| Singapore                | Asia      |  1992|  75.78800|     3235865|   24769.8912| NA          |
| Singapore                | Asia      |  1997|  77.15800|     3802309|   33519.4766| NA          |
| Singapore                | Asia      |  2002|  78.77000|     4197776|   36023.1054| NA          |
| Singapore                | Asia      |  2007|  79.97200|     4553009|   47143.1796| NA          |
| Slovak Republic          | Europe    |  1952|  64.36000|     3558137|    5074.6591| NA          |
| Slovak Republic          | Europe    |  1957|  67.45000|     3844277|    6093.2630| NA          |
| Slovak Republic          | Europe    |  1962|  70.33000|     4237384|    7481.1076| NA          |
| Slovak Republic          | Europe    |  1967|  70.98000|     4442238|    8412.9024| NA          |
| Slovak Republic          | Europe    |  1972|  70.35000|     4593433|    9674.1676| NA          |
| Slovak Republic          | Europe    |  1977|  70.45000|     4827803|   10922.6640| NA          |
| Slovak Republic          | Europe    |  1982|  70.80000|     5048043|   11348.5459| NA          |
| Slovak Republic          | Europe    |  1987|  71.08000|     5199318|   12037.2676| NA          |
| Slovak Republic          | Europe    |  1992|  71.38000|     5302888|    9498.4677| NA          |
| Slovak Republic          | Europe    |  1997|  72.71000|     5383010|   12126.2306| NA          |
| Slovak Republic          | Europe    |  2002|  73.80000|     5410052|   13638.7784| NA          |
| Slovak Republic          | Europe    |  2007|  74.66300|     5447502|   18678.3144| NA          |
| Slovenia                 | Europe    |  1952|  65.57000|     1489518|    4215.0417| NA          |
| Slovenia                 | Europe    |  1957|  67.85000|     1533070|    5862.2766| NA          |
| Slovenia                 | Europe    |  1962|  69.15000|     1582962|    7402.3034| NA          |
| Slovenia                 | Europe    |  1967|  69.18000|     1646912|    9405.4894| NA          |
| Slovenia                 | Europe    |  1972|  69.82000|     1694510|   12383.4862| NA          |
| Slovenia                 | Europe    |  1977|  70.97000|     1746919|   15277.0302| NA          |
| Slovenia                 | Europe    |  1982|  71.06300|     1861252|   17866.7218| NA          |
| Slovenia                 | Europe    |  1987|  72.25000|     1945870|   18678.5349| NA          |
| Slovenia                 | Europe    |  1992|  73.64000|     1999210|   14214.7168| NA          |
| Slovenia                 | Europe    |  1997|  75.13000|     2011612|   17161.1073| NA          |
| Slovenia                 | Europe    |  2002|  76.66000|     2011497|   20660.0194| NA          |
| Slovenia                 | Europe    |  2007|  77.92600|     2009245|   25768.2576| NA          |
| Somalia                  | Africa    |  1952|  32.97800|     2526994|    1135.7498| Mogadishu   |
| Somalia                  | Africa    |  1957|  34.97700|     2780415|    1258.1474| Mogadishu   |
| Somalia                  | Africa    |  1962|  36.98100|     3080153|    1369.4883| Mogadishu   |
| Somalia                  | Africa    |  1967|  38.97700|     3428839|    1284.7332| Mogadishu   |
| Somalia                  | Africa    |  1972|  40.97300|     3840161|    1254.5761| Mogadishu   |
| Somalia                  | Africa    |  1977|  41.97400|     4353666|    1450.9925| Mogadishu   |
| Somalia                  | Africa    |  1982|  42.95500|     5828892|    1176.8070| Mogadishu   |
| Somalia                  | Africa    |  1987|  44.50100|     6921858|    1093.2450| Mogadishu   |
| Somalia                  | Africa    |  1992|  39.65800|     6099799|     926.9603| Mogadishu   |
| Somalia                  | Africa    |  1997|  43.79500|     6633514|     930.5964| Mogadishu   |
| Somalia                  | Africa    |  2002|  45.93600|     7753310|     882.0818| Mogadishu   |
| Somalia                  | Africa    |  2007|  48.15900|     9118773|     926.1411| Mogadishu   |
| South Africa             | Africa    |  1952|  45.00900|    14264935|    4725.2955| NA          |
| South Africa             | Africa    |  1957|  47.98500|    16151549|    5487.1042| NA          |
| South Africa             | Africa    |  1962|  49.95100|    18356657|    5768.7297| NA          |
| South Africa             | Africa    |  1967|  51.92700|    20997321|    7114.4780| NA          |
| South Africa             | Africa    |  1972|  53.69600|    23935810|    7765.9626| NA          |
| South Africa             | Africa    |  1977|  55.52700|    27129932|    8028.6514| NA          |
| South Africa             | Africa    |  1982|  58.16100|    31140029|    8568.2662| NA          |
| South Africa             | Africa    |  1987|  60.83400|    35933379|    7825.8234| NA          |
| South Africa             | Africa    |  1992|  61.88800|    39964159|    7225.0693| NA          |
| South Africa             | Africa    |  1997|  60.23600|    42835005|    7479.1882| NA          |
| South Africa             | Africa    |  2002|  53.36500|    44433622|    7710.9464| NA          |
| South Africa             | Africa    |  2007|  49.33900|    43997828|    9269.6578| NA          |
| Spain                    | Europe    |  1952|  64.94000|    28549870|    3834.0347| NA          |
| Spain                    | Europe    |  1957|  66.66000|    29841614|    4564.8024| NA          |
| Spain                    | Europe    |  1962|  69.69000|    31158061|    5693.8439| NA          |
| Spain                    | Europe    |  1967|  71.44000|    32850275|    7993.5123| NA          |
| Spain                    | Europe    |  1972|  73.06000|    34513161|   10638.7513| NA          |
| Spain                    | Europe    |  1977|  74.39000|    36439000|   13236.9212| NA          |
| Spain                    | Europe    |  1982|  76.30000|    37983310|   13926.1700| NA          |
| Spain                    | Europe    |  1987|  76.90000|    38880702|   15764.9831| NA          |
| Spain                    | Europe    |  1992|  77.57000|    39549438|   18603.0645| NA          |
| Spain                    | Europe    |  1997|  78.77000|    39855442|   20445.2990| NA          |
| Spain                    | Europe    |  2002|  79.78000|    40152517|   24835.4717| NA          |
| Spain                    | Europe    |  2007|  80.94100|    40448191|   28821.0637| NA          |
| Sri Lanka                | Asia      |  1952|  57.59300|     7982342|    1083.5320| NA          |
| Sri Lanka                | Asia      |  1957|  61.45600|     9128546|    1072.5466| NA          |
| Sri Lanka                | Asia      |  1962|  62.19200|    10421936|    1074.4720| NA          |
| Sri Lanka                | Asia      |  1967|  64.26600|    11737396|    1135.5143| NA          |
| Sri Lanka                | Asia      |  1972|  65.04200|    13016733|    1213.3955| NA          |
| Sri Lanka                | Asia      |  1977|  65.94900|    14116836|    1348.7757| NA          |
| Sri Lanka                | Asia      |  1982|  68.75700|    15410151|    1648.0798| NA          |
| Sri Lanka                | Asia      |  1987|  69.01100|    16495304|    1876.7668| NA          |
| Sri Lanka                | Asia      |  1992|  70.37900|    17587060|    2153.7392| NA          |
| Sri Lanka                | Asia      |  1997|  70.45700|    18698655|    2664.4773| NA          |
| Sri Lanka                | Asia      |  2002|  70.81500|    19576783|    3015.3788| NA          |
| Sri Lanka                | Asia      |  2007|  72.39600|    20378239|    3970.0954| NA          |
| Sudan                    | Africa    |  1952|  38.63500|     8504667|    1615.9911| NA          |
| Sudan                    | Africa    |  1957|  39.62400|     9753392|    1770.3371| NA          |
| Sudan                    | Africa    |  1962|  40.87000|    11183227|    1959.5938| NA          |
| Sudan                    | Africa    |  1967|  42.85800|    12716129|    1687.9976| NA          |
| Sudan                    | Africa    |  1972|  45.08300|    14597019|    1659.6528| NA          |
| Sudan                    | Africa    |  1977|  47.80000|    17104986|    2202.9884| NA          |
| Sudan                    | Africa    |  1982|  50.33800|    20367053|    1895.5441| NA          |
| Sudan                    | Africa    |  1987|  51.74400|    24725960|    1507.8192| NA          |
| Sudan                    | Africa    |  1992|  53.55600|    28227588|    1492.1970| NA          |
| Sudan                    | Africa    |  1997|  55.37300|    32160729|    1632.2108| NA          |
| Sudan                    | Africa    |  2002|  56.36900|    37090298|    1993.3983| NA          |
| Sudan                    | Africa    |  2007|  58.55600|    42292929|    2602.3950| NA          |
| Swaziland                | Africa    |  1952|  41.40700|      290243|    1148.3766| NA          |
| Swaziland                | Africa    |  1957|  43.42400|      326741|    1244.7084| NA          |
| Swaziland                | Africa    |  1962|  44.99200|      370006|    1856.1821| NA          |
| Swaziland                | Africa    |  1967|  46.63300|      420690|    2613.1017| NA          |
| Swaziland                | Africa    |  1972|  49.55200|      480105|    3364.8366| NA          |
| Swaziland                | Africa    |  1977|  52.53700|      551425|    3781.4106| NA          |
| Swaziland                | Africa    |  1982|  55.56100|      649901|    3895.3840| NA          |
| Swaziland                | Africa    |  1987|  57.67800|      779348|    3984.8398| NA          |
| Swaziland                | Africa    |  1992|  58.47400|      962344|    3553.0224| NA          |
| Swaziland                | Africa    |  1997|  54.28900|     1054486|    3876.7685| NA          |
| Swaziland                | Africa    |  2002|  43.86900|     1130269|    4128.1169| NA          |
| Swaziland                | Africa    |  2007|  39.61300|     1133066|    4513.4806| NA          |
| Sweden                   | Europe    |  1952|  71.86000|     7124673|    8527.8447| NA          |
| Sweden                   | Europe    |  1957|  72.49000|     7363802|    9911.8782| NA          |
| Sweden                   | Europe    |  1962|  73.37000|     7561588|   12329.4419| NA          |
| Sweden                   | Europe    |  1967|  74.16000|     7867931|   15258.2970| NA          |
| Sweden                   | Europe    |  1972|  74.72000|     8122293|   17832.0246| NA          |
| Sweden                   | Europe    |  1977|  75.44000|     8251648|   18855.7252| NA          |
| Sweden                   | Europe    |  1982|  76.42000|     8325260|   20667.3812| NA          |
| Sweden                   | Europe    |  1987|  77.19000|     8421403|   23586.9293| NA          |
| Sweden                   | Europe    |  1992|  78.16000|     8718867|   23880.0168| NA          |
| Sweden                   | Europe    |  1997|  79.39000|     8897619|   25266.5950| NA          |
| Sweden                   | Europe    |  2002|  80.04000|     8954175|   29341.6309| NA          |
| Sweden                   | Europe    |  2007|  80.88400|     9031088|   33859.7484| NA          |
| Switzerland              | Europe    |  1952|  69.62000|     4815000|   14734.2327| NA          |
| Switzerland              | Europe    |  1957|  70.56000|     5126000|   17909.4897| NA          |
| Switzerland              | Europe    |  1962|  71.32000|     5666000|   20431.0927| NA          |
| Switzerland              | Europe    |  1967|  72.77000|     6063000|   22966.1443| NA          |
| Switzerland              | Europe    |  1972|  73.78000|     6401400|   27195.1130| NA          |
| Switzerland              | Europe    |  1977|  75.39000|     6316424|   26982.2905| NA          |
| Switzerland              | Europe    |  1982|  76.21000|     6468126|   28397.7151| NA          |
| Switzerland              | Europe    |  1987|  77.41000|     6649942|   30281.7046| NA          |
| Switzerland              | Europe    |  1992|  78.03000|     6995447|   31871.5303| NA          |
| Switzerland              | Europe    |  1997|  79.37000|     7193761|   32135.3230| NA          |
| Switzerland              | Europe    |  2002|  80.62000|     7361757|   34480.9577| NA          |
| Switzerland              | Europe    |  2007|  81.70100|     7554661|   37506.4191| NA          |
| Syria                    | Asia      |  1952|  45.88300|     3661549|    1643.4854| NA          |
| Syria                    | Asia      |  1957|  48.28400|     4149908|    2117.2349| NA          |
| Syria                    | Asia      |  1962|  50.30500|     4834621|    2193.0371| NA          |
| Syria                    | Asia      |  1967|  53.65500|     5680812|    1881.9236| NA          |
| Syria                    | Asia      |  1972|  57.29600|     6701172|    2571.4230| NA          |
| Syria                    | Asia      |  1977|  61.19500|     7932503|    3195.4846| NA          |
| Syria                    | Asia      |  1982|  64.59000|     9410494|    3761.8377| NA          |
| Syria                    | Asia      |  1987|  66.97400|    11242847|    3116.7743| NA          |
| Syria                    | Asia      |  1992|  69.24900|    13219062|    3340.5428| NA          |
| Syria                    | Asia      |  1997|  71.52700|    15081016|    4014.2390| NA          |
| Syria                    | Asia      |  2002|  73.05300|    17155814|    4090.9253| NA          |
| Syria                    | Asia      |  2007|  74.14300|    19314747|    4184.5481| NA          |
| Taiwan                   | Asia      |  1952|  58.50000|     8550362|    1206.9479| NA          |
| Taiwan                   | Asia      |  1957|  62.40000|    10164215|    1507.8613| NA          |
| Taiwan                   | Asia      |  1962|  65.20000|    11918938|    1822.8790| NA          |
| Taiwan                   | Asia      |  1967|  67.50000|    13648692|    2643.8587| NA          |
| Taiwan                   | Asia      |  1972|  69.39000|    15226039|    4062.5239| NA          |
| Taiwan                   | Asia      |  1977|  70.59000|    16785196|    5596.5198| NA          |
| Taiwan                   | Asia      |  1982|  72.16000|    18501390|    7426.3548| NA          |
| Taiwan                   | Asia      |  1987|  73.40000|    19757799|   11054.5618| NA          |
| Taiwan                   | Asia      |  1992|  74.26000|    20686918|   15215.6579| NA          |
| Taiwan                   | Asia      |  1997|  75.25000|    21628605|   20206.8210| NA          |
| Taiwan                   | Asia      |  2002|  76.99000|    22454239|   23235.4233| NA          |
| Taiwan                   | Asia      |  2007|  78.40000|    23174294|   28718.2768| NA          |
| Tanzania                 | Africa    |  1952|  41.21500|     8322925|     716.6501| NA          |
| Tanzania                 | Africa    |  1957|  42.97400|     9452826|     698.5356| NA          |
| Tanzania                 | Africa    |  1962|  44.24600|    10863958|     722.0038| NA          |
| Tanzania                 | Africa    |  1967|  45.75700|    12607312|     848.2187| NA          |
| Tanzania                 | Africa    |  1972|  47.62000|    14706593|     915.9851| NA          |
| Tanzania                 | Africa    |  1977|  49.91900|    17129565|     962.4923| NA          |
| Tanzania                 | Africa    |  1982|  50.60800|    19844382|     874.2426| NA          |
| Tanzania                 | Africa    |  1987|  51.53500|    23040630|     831.8221| NA          |
| Tanzania                 | Africa    |  1992|  50.44000|    26605473|     825.6825| NA          |
| Tanzania                 | Africa    |  1997|  48.46600|    30686889|     789.1862| NA          |
| Tanzania                 | Africa    |  2002|  49.65100|    34593779|     899.0742| NA          |
| Tanzania                 | Africa    |  2007|  52.51700|    38139640|    1107.4822| NA          |
| Thailand                 | Asia      |  1952|  50.84800|    21289402|     757.7974| Bangkok     |
| Thailand                 | Asia      |  1957|  53.63000|    25041917|     793.5774| Bangkok     |
| Thailand                 | Asia      |  1962|  56.06100|    29263397|    1002.1992| Bangkok     |
| Thailand                 | Asia      |  1967|  58.28500|    34024249|    1295.4607| Bangkok     |
| Thailand                 | Asia      |  1972|  60.40500|    39276153|    1524.3589| Bangkok     |
| Thailand                 | Asia      |  1977|  62.49400|    44148285|    1961.2246| Bangkok     |
| Thailand                 | Asia      |  1982|  64.59700|    48827160|    2393.2198| Bangkok     |
| Thailand                 | Asia      |  1987|  66.08400|    52910342|    2982.6538| Bangkok     |
| Thailand                 | Asia      |  1992|  67.29800|    56667095|    4616.8965| Bangkok     |
| Thailand                 | Asia      |  1997|  67.52100|    60216677|    5852.6255| Bangkok     |
| Thailand                 | Asia      |  2002|  68.56400|    62806748|    5913.1875| Bangkok     |
| Thailand                 | Asia      |  2007|  70.61600|    65068149|    7458.3963| Bangkok     |
| Togo                     | Africa    |  1952|  38.59600|     1219113|     859.8087| NA          |
| Togo                     | Africa    |  1957|  41.20800|     1357445|     925.9083| NA          |
| Togo                     | Africa    |  1962|  43.92200|     1528098|    1067.5348| NA          |
| Togo                     | Africa    |  1967|  46.76900|     1735550|    1477.5968| NA          |
| Togo                     | Africa    |  1972|  49.75900|     2056351|    1649.6602| NA          |
| Togo                     | Africa    |  1977|  52.88700|     2308582|    1532.7770| NA          |
| Togo                     | Africa    |  1982|  55.47100|     2644765|    1344.5780| NA          |
| Togo                     | Africa    |  1987|  56.94100|     3154264|    1202.2014| NA          |
| Togo                     | Africa    |  1992|  58.06100|     3747553|    1034.2989| NA          |
| Togo                     | Africa    |  1997|  58.39000|     4320890|     982.2869| NA          |
| Togo                     | Africa    |  2002|  57.56100|     4977378|     886.2206| NA          |
| Togo                     | Africa    |  2007|  58.42000|     5701579|     882.9699| NA          |
| Trinidad and Tobago      | Americas  |  1952|  59.10000|      662850|    3023.2719| NA          |
| Trinidad and Tobago      | Americas  |  1957|  61.80000|      764900|    4100.3934| NA          |
| Trinidad and Tobago      | Americas  |  1962|  64.90000|      887498|    4997.5240| NA          |
| Trinidad and Tobago      | Americas  |  1967|  65.40000|      960155|    5621.3685| NA          |
| Trinidad and Tobago      | Americas  |  1972|  65.90000|      975199|    6619.5514| NA          |
| Trinidad and Tobago      | Americas  |  1977|  68.30000|     1039009|    7899.5542| NA          |
| Trinidad and Tobago      | Americas  |  1982|  68.83200|     1116479|    9119.5286| NA          |
| Trinidad and Tobago      | Americas  |  1987|  69.58200|     1191336|    7388.5978| NA          |
| Trinidad and Tobago      | Americas  |  1992|  69.86200|     1183669|    7370.9909| NA          |
| Trinidad and Tobago      | Americas  |  1997|  69.46500|     1138101|    8792.5731| NA          |
| Trinidad and Tobago      | Americas  |  2002|  68.97600|     1101832|   11460.6002| NA          |
| Trinidad and Tobago      | Americas  |  2007|  69.81900|     1056608|   18008.5092| NA          |
| Tunisia                  | Africa    |  1952|  44.60000|     3647735|    1468.4756| NA          |
| Tunisia                  | Africa    |  1957|  47.10000|     3950849|    1395.2325| NA          |
| Tunisia                  | Africa    |  1962|  49.57900|     4286552|    1660.3032| NA          |
| Tunisia                  | Africa    |  1967|  52.05300|     4786986|    1932.3602| NA          |
| Tunisia                  | Africa    |  1972|  55.60200|     5303507|    2753.2860| NA          |
| Tunisia                  | Africa    |  1977|  59.83700|     6005061|    3120.8768| NA          |
| Tunisia                  | Africa    |  1982|  64.04800|     6734098|    3560.2332| NA          |
| Tunisia                  | Africa    |  1987|  66.89400|     7724976|    3810.4193| NA          |
| Tunisia                  | Africa    |  1992|  70.00100|     8523077|    4332.7202| NA          |
| Tunisia                  | Africa    |  1997|  71.97300|     9231669|    4876.7986| NA          |
| Tunisia                  | Africa    |  2002|  73.04200|     9770575|    5722.8957| NA          |
| Tunisia                  | Africa    |  2007|  73.92300|    10276158|    7092.9230| NA          |
| Turkey                   | Europe    |  1952|  43.58500|    22235677|    1969.1010| Ankara      |
| Turkey                   | Europe    |  1957|  48.07900|    25670939|    2218.7543| Ankara      |
| Turkey                   | Europe    |  1962|  52.09800|    29788695|    2322.8699| Ankara      |
| Turkey                   | Europe    |  1967|  54.33600|    33411317|    2826.3564| Ankara      |
| Turkey                   | Europe    |  1972|  57.00500|    37492953|    3450.6964| Ankara      |
| Turkey                   | Europe    |  1977|  59.50700|    42404033|    4269.1223| Ankara      |
| Turkey                   | Europe    |  1982|  61.03600|    47328791|    4241.3563| Ankara      |
| Turkey                   | Europe    |  1987|  63.10800|    52881328|    5089.0437| Ankara      |
| Turkey                   | Europe    |  1992|  66.14600|    58179144|    5678.3483| Ankara      |
| Turkey                   | Europe    |  1997|  68.83500|    63047647|    6601.4299| Ankara      |
| Turkey                   | Europe    |  2002|  70.84500|    67308928|    6508.0857| Ankara      |
| Turkey                   | Europe    |  2007|  71.77700|    71158647|    8458.2764| Ankara      |
| Uganda                   | Africa    |  1952|  39.97800|     5824797|     734.7535| NA          |
| Uganda                   | Africa    |  1957|  42.57100|     6675501|     774.3711| NA          |
| Uganda                   | Africa    |  1962|  45.34400|     7688797|     767.2717| NA          |
| Uganda                   | Africa    |  1967|  48.05100|     8900294|     908.9185| NA          |
| Uganda                   | Africa    |  1972|  51.01600|    10190285|     950.7359| NA          |
| Uganda                   | Africa    |  1977|  50.35000|    11457758|     843.7331| NA          |
| Uganda                   | Africa    |  1982|  49.84900|    12939400|     682.2662| NA          |
| Uganda                   | Africa    |  1987|  51.50900|    15283050|     617.7244| NA          |
| Uganda                   | Africa    |  1992|  48.82500|    18252190|     644.1708| NA          |
| Uganda                   | Africa    |  1997|  44.57800|    21210254|     816.5591| NA          |
| Uganda                   | Africa    |  2002|  47.81300|    24739869|     927.7210| NA          |
| Uganda                   | Africa    |  2007|  51.54200|    29170398|    1056.3801| NA          |
| United Kingdom           | Europe    |  1952|  69.18000|    50430000|    9979.5085| NA          |
| United Kingdom           | Europe    |  1957|  70.42000|    51430000|   11283.1779| NA          |
| United Kingdom           | Europe    |  1962|  70.76000|    53292000|   12477.1771| NA          |
| United Kingdom           | Europe    |  1967|  71.36000|    54959000|   14142.8509| NA          |
| United Kingdom           | Europe    |  1972|  72.01000|    56079000|   15895.1164| NA          |
| United Kingdom           | Europe    |  1977|  72.76000|    56179000|   17428.7485| NA          |
| United Kingdom           | Europe    |  1982|  74.04000|    56339704|   18232.4245| NA          |
| United Kingdom           | Europe    |  1987|  75.00700|    56981620|   21664.7877| NA          |
| United Kingdom           | Europe    |  1992|  76.42000|    57866349|   22705.0925| NA          |
| United Kingdom           | Europe    |  1997|  77.21800|    58808266|   26074.5314| NA          |
| United Kingdom           | Europe    |  2002|  78.47100|    59912431|   29478.9992| NA          |
| United Kingdom           | Europe    |  2007|  79.42500|    60776238|   33203.2613| NA          |
| United States            | Americas  |  1952|  68.44000|   157553000|   13990.4821| Washington  |
| United States            | Americas  |  1957|  69.49000|   171984000|   14847.1271| Washington  |
| United States            | Americas  |  1962|  70.21000|   186538000|   16173.1459| Washington  |
| United States            | Americas  |  1967|  70.76000|   198712000|   19530.3656| Washington  |
| United States            | Americas  |  1972|  71.34000|   209896000|   21806.0359| Washington  |
| United States            | Americas  |  1977|  73.38000|   220239000|   24072.6321| Washington  |
| United States            | Americas  |  1982|  74.65000|   232187835|   25009.5591| Washington  |
| United States            | Americas  |  1987|  75.02000|   242803533|   29884.3504| Washington  |
| United States            | Americas  |  1992|  76.09000|   256894189|   32003.9322| Washington  |
| United States            | Americas  |  1997|  76.81000|   272911760|   35767.4330| Washington  |
| United States            | Americas  |  2002|  77.31000|   287675526|   39097.0995| Washington  |
| United States            | Americas  |  2007|  78.24200|   301139947|   42951.6531| Washington  |
| Uruguay                  | Americas  |  1952|  66.07100|     2252965|    5716.7667| Montevideo  |
| Uruguay                  | Americas  |  1957|  67.04400|     2424959|    6150.7730| Montevideo  |
| Uruguay                  | Americas  |  1962|  68.25300|     2598466|    5603.3577| Montevideo  |
| Uruguay                  | Americas  |  1967|  68.46800|     2748579|    5444.6196| Montevideo  |
| Uruguay                  | Americas  |  1972|  68.67300|     2829526|    5703.4089| Montevideo  |
| Uruguay                  | Americas  |  1977|  69.48100|     2873520|    6504.3397| Montevideo  |
| Uruguay                  | Americas  |  1982|  70.80500|     2953997|    6920.2231| Montevideo  |
| Uruguay                  | Americas  |  1987|  71.91800|     3045153|    7452.3990| Montevideo  |
| Uruguay                  | Americas  |  1992|  72.75200|     3149262|    8137.0048| Montevideo  |
| Uruguay                  | Americas  |  1997|  74.22300|     3262838|    9230.2407| Montevideo  |
| Uruguay                  | Americas  |  2002|  75.30700|     3363085|    7727.0020| Montevideo  |
| Uruguay                  | Americas  |  2007|  76.38400|     3447496|   10611.4630| Montevideo  |
| Venezuela                | Americas  |  1952|  55.08800|     5439568|    7689.7998| NA          |
| Venezuela                | Americas  |  1957|  57.90700|     6702668|    9802.4665| NA          |
| Venezuela                | Americas  |  1962|  60.77000|     8143375|    8422.9742| NA          |
| Venezuela                | Americas  |  1967|  63.47900|     9709552|    9541.4742| NA          |
| Venezuela                | Americas  |  1972|  65.71200|    11515649|   10505.2597| NA          |
| Venezuela                | Americas  |  1977|  67.45600|    13503563|   13143.9510| NA          |
| Venezuela                | Americas  |  1982|  68.55700|    15620766|   11152.4101| NA          |
| Venezuela                | Americas  |  1987|  70.19000|    17910182|    9883.5846| NA          |
| Venezuela                | Americas  |  1992|  71.15000|    20265563|   10733.9263| NA          |
| Venezuela                | Americas  |  1997|  72.14600|    22374398|   10165.4952| NA          |
| Venezuela                | Americas  |  2002|  72.76600|    24287670|    8605.0478| NA          |
| Venezuela                | Americas  |  2007|  73.74700|    26084662|   11415.8057| NA          |
| Vietnam                  | Asia      |  1952|  40.41200|    26246839|     605.0665| NA          |
| Vietnam                  | Asia      |  1957|  42.88700|    28998543|     676.2854| NA          |
| Vietnam                  | Asia      |  1962|  45.36300|    33796140|     772.0492| NA          |
| Vietnam                  | Asia      |  1967|  47.83800|    39463910|     637.1233| NA          |
| Vietnam                  | Asia      |  1972|  50.25400|    44655014|     699.5016| NA          |
| Vietnam                  | Asia      |  1977|  55.76400|    50533506|     713.5371| NA          |
| Vietnam                  | Asia      |  1982|  58.81600|    56142181|     707.2358| NA          |
| Vietnam                  | Asia      |  1987|  62.82000|    62826491|     820.7994| NA          |
| Vietnam                  | Asia      |  1992|  67.66200|    69940728|     989.0231| NA          |
| Vietnam                  | Asia      |  1997|  70.67200|    76048996|    1385.8968| NA          |
| Vietnam                  | Asia      |  2002|  73.01700|    80908147|    1764.4567| NA          |
| Vietnam                  | Asia      |  2007|  74.24900|    85262356|    2441.5764| NA          |
| West Bank and Gaza       | Asia      |  1952|  43.16000|     1030585|    1515.5923| NA          |
| West Bank and Gaza       | Asia      |  1957|  45.67100|     1070439|    1827.0677| NA          |
| West Bank and Gaza       | Asia      |  1962|  48.12700|     1133134|    2198.9563| NA          |
| West Bank and Gaza       | Asia      |  1967|  51.63100|     1142636|    2649.7150| NA          |
| West Bank and Gaza       | Asia      |  1972|  56.53200|     1089572|    3133.4093| NA          |
| West Bank and Gaza       | Asia      |  1977|  60.76500|     1261091|    3682.8315| NA          |
| West Bank and Gaza       | Asia      |  1982|  64.40600|     1425876|    4336.0321| NA          |
| West Bank and Gaza       | Asia      |  1987|  67.04600|     1691210|    5107.1974| NA          |
| West Bank and Gaza       | Asia      |  1992|  69.71800|     2104779|    6017.6548| NA          |
| West Bank and Gaza       | Asia      |  1997|  71.09600|     2826046|    7110.6676| NA          |
| West Bank and Gaza       | Asia      |  2002|  72.37000|     3389578|    4515.4876| NA          |
| West Bank and Gaza       | Asia      |  2007|  73.42200|     4018332|    3025.3498| NA          |
| Yemen, Rep.              | Asia      |  1952|  32.54800|     4963829|     781.7176| NA          |
| Yemen, Rep.              | Asia      |  1957|  33.97000|     5498090|     804.8305| NA          |
| Yemen, Rep.              | Asia      |  1962|  35.18000|     6120081|     825.6232| NA          |
| Yemen, Rep.              | Asia      |  1967|  36.98400|     6740785|     862.4421| NA          |
| Yemen, Rep.              | Asia      |  1972|  39.84800|     7407075|    1265.0470| NA          |
| Yemen, Rep.              | Asia      |  1977|  44.17500|     8403990|    1829.7652| NA          |
| Yemen, Rep.              | Asia      |  1982|  49.11300|     9657618|    1977.5570| NA          |
| Yemen, Rep.              | Asia      |  1987|  52.92200|    11219340|    1971.7415| NA          |
| Yemen, Rep.              | Asia      |  1992|  55.59900|    13367997|    1879.4967| NA          |
| Yemen, Rep.              | Asia      |  1997|  58.02000|    15826497|    2117.4845| NA          |
| Yemen, Rep.              | Asia      |  2002|  60.30800|    18701257|    2234.8208| NA          |
| Yemen, Rep.              | Asia      |  2007|  62.69800|    22211743|    2280.7699| NA          |
| Zambia                   | Africa    |  1952|  42.03800|     2672000|    1147.3888| NA          |
| Zambia                   | Africa    |  1957|  44.07700|     3016000|    1311.9568| NA          |
| Zambia                   | Africa    |  1962|  46.02300|     3421000|    1452.7258| NA          |
| Zambia                   | Africa    |  1967|  47.76800|     3900000|    1777.0773| NA          |
| Zambia                   | Africa    |  1972|  50.10700|     4506497|    1773.4983| NA          |
| Zambia                   | Africa    |  1977|  51.38600|     5216550|    1588.6883| NA          |
| Zambia                   | Africa    |  1982|  51.82100|     6100407|    1408.6786| NA          |
| Zambia                   | Africa    |  1987|  50.82100|     7272406|    1213.3151| NA          |
| Zambia                   | Africa    |  1992|  46.10000|     8381163|    1210.8846| NA          |
| Zambia                   | Africa    |  1997|  40.23800|     9417789|    1071.3538| NA          |
| Zambia                   | Africa    |  2002|  39.19300|    10595811|    1071.6139| NA          |
| Zambia                   | Africa    |  2007|  42.38400|    11746035|    1271.2116| NA          |
| Zimbabwe                 | Africa    |  1952|  48.45100|     3080907|     406.8841| Harare      |
| Zimbabwe                 | Africa    |  1957|  50.46900|     3646340|     518.7643| Harare      |
| Zimbabwe                 | Africa    |  1962|  52.35800|     4277736|     527.2722| Harare      |
| Zimbabwe                 | Africa    |  1967|  53.99500|     4995432|     569.7951| Harare      |
| Zimbabwe                 | Africa    |  1972|  55.63500|     5861135|     799.3622| Harare      |
| Zimbabwe                 | Africa    |  1977|  57.67400|     6642107|     685.5877| Harare      |
| Zimbabwe                 | Africa    |  1982|  60.36300|     7636524|     788.8550| Harare      |
| Zimbabwe                 | Africa    |  1987|  62.35100|     9216418|     706.1573| Harare      |
| Zimbabwe                 | Africa    |  1992|  60.37700|    10704340|     693.4208| Harare      |
| Zimbabwe                 | Africa    |  1997|  46.80900|    11404948|     792.4500| Harare      |
| Zimbabwe                 | Africa    |  2002|  39.98900|    11926563|     672.0386| Harare      |
| Zimbabwe                 | Africa    |  2007|  43.48700|    12311143|     469.7093| Harare      |
| Russia                   | NA        |    NA|        NA|          NA|           NA| Moscow      |

The countries from both gapminder and countries\_18 are kept when doing full join, so the resulting table consists of 1705 rows (one additional country that is not in gapminder is added) and 7 columns (6 from gapminder and 1 from countries\_18). If there is no information for a specific row in either of the datasets (eg Afghanistan is not in countries\_18), then NA is added for the column from a second dataset (in this case, `capital`).

``` r
joined_semi <- semi_join(gapminder, countries_18)
```

    ## Joining, by = "country"

    ## Warning: Column `country` joining factor and character vector, coercing
    ## into character vector

``` r
str(joined_semi)
```

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    204 obs. of  6 variables:
    ##  $ country  : Factor w/ 142 levels "Afghanistan",..: 6 6 6 6 6 6 6 6 6 6 ...
    ##  $ continent: Factor w/ 5 levels "Africa","Americas",..: 5 5 5 5 5 5 5 5 5 5 ...
    ##  $ year     : int  1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
    ##  $ lifeExp  : num  69.1 70.3 70.9 71.1 71.9 ...
    ##  $ pop      : int  8691212 9712569 10794968 11872264 13177000 14074100 15184200 16257249 17481977 18565243 ...
    ##  $ gdpPercap: num  10040 10950 12217 14526 16789 ...

``` r
levels(as.factor(joined_semi$country))
```

    ##   [1] "Afghanistan"              "Albania"                 
    ##   [3] "Algeria"                  "Angola"                  
    ##   [5] "Argentina"                "Australia"               
    ##   [7] "Austria"                  "Bahrain"                 
    ##   [9] "Bangladesh"               "Belgium"                 
    ##  [11] "Benin"                    "Bolivia"                 
    ##  [13] "Bosnia and Herzegovina"   "Botswana"                
    ##  [15] "Brazil"                   "Bulgaria"                
    ##  [17] "Burkina Faso"             "Burundi"                 
    ##  [19] "Cambodia"                 "Cameroon"                
    ##  [21] "Canada"                   "Central African Republic"
    ##  [23] "Chad"                     "Chile"                   
    ##  [25] "China"                    "Colombia"                
    ##  [27] "Comoros"                  "Congo, Dem. Rep."        
    ##  [29] "Congo, Rep."              "Costa Rica"              
    ##  [31] "Cote d'Ivoire"            "Croatia"                 
    ##  [33] "Cuba"                     "Czech Republic"          
    ##  [35] "Denmark"                  "Djibouti"                
    ##  [37] "Dominican Republic"       "Ecuador"                 
    ##  [39] "Egypt"                    "El Salvador"             
    ##  [41] "Equatorial Guinea"        "Eritrea"                 
    ##  [43] "Ethiopia"                 "Finland"                 
    ##  [45] "France"                   "Gabon"                   
    ##  [47] "Gambia"                   "Germany"                 
    ##  [49] "Ghana"                    "Greece"                  
    ##  [51] "Guatemala"                "Guinea"                  
    ##  [53] "Guinea-Bissau"            "Haiti"                   
    ##  [55] "Honduras"                 "Hong Kong, China"        
    ##  [57] "Hungary"                  "Iceland"                 
    ##  [59] "India"                    "Indonesia"               
    ##  [61] "Iran"                     "Iraq"                    
    ##  [63] "Ireland"                  "Israel"                  
    ##  [65] "Italy"                    "Jamaica"                 
    ##  [67] "Japan"                    "Jordan"                  
    ##  [69] "Kenya"                    "Korea, Dem. Rep."        
    ##  [71] "Korea, Rep."              "Kuwait"                  
    ##  [73] "Lebanon"                  "Lesotho"                 
    ##  [75] "Liberia"                  "Libya"                   
    ##  [77] "Madagascar"               "Malawi"                  
    ##  [79] "Malaysia"                 "Mali"                    
    ##  [81] "Mauritania"               "Mauritius"               
    ##  [83] "Mexico"                   "Mongolia"                
    ##  [85] "Montenegro"               "Morocco"                 
    ##  [87] "Mozambique"               "Myanmar"                 
    ##  [89] "Namibia"                  "Nepal"                   
    ##  [91] "Netherlands"              "New Zealand"             
    ##  [93] "Nicaragua"                "Niger"                   
    ##  [95] "Nigeria"                  "Norway"                  
    ##  [97] "Oman"                     "Pakistan"                
    ##  [99] "Panama"                   "Paraguay"                
    ## [101] "Peru"                     "Philippines"             
    ## [103] "Poland"                   "Portugal"                
    ## [105] "Puerto Rico"              "Reunion"                 
    ## [107] "Romania"                  "Rwanda"                  
    ## [109] "Sao Tome and Principe"    "Saudi Arabia"            
    ## [111] "Senegal"                  "Serbia"                  
    ## [113] "Sierra Leone"             "Singapore"               
    ## [115] "Slovak Republic"          "Slovenia"                
    ## [117] "Somalia"                  "South Africa"            
    ## [119] "Spain"                    "Sri Lanka"               
    ## [121] "Sudan"                    "Swaziland"               
    ## [123] "Sweden"                   "Switzerland"             
    ## [125] "Syria"                    "Taiwan"                  
    ## [127] "Tanzania"                 "Thailand"                
    ## [129] "Togo"                     "Trinidad and Tobago"     
    ## [131] "Tunisia"                  "Turkey"                  
    ## [133] "Uganda"                   "United Kingdom"          
    ## [135] "United States"            "Uruguay"                 
    ## [137] "Venezuela"                "Vietnam"                 
    ## [139] "West Bank and Gaza"       "Yemen, Rep."             
    ## [141] "Zambia"                   "Zimbabwe"

``` r
levels(as.factor(joined_semi$year))
```

    ##  [1] "1952" "1957" "1962" "1967" "1972" "1977" "1982" "1987" "1992" "1997"
    ## [11] "2002" "2007"

``` r
knitr::kable(joined_semi)
```

| country       | continent |  year|   lifeExp|         pop|   gdpPercap|
|:--------------|:----------|-----:|---------:|-----------:|-----------:|
| Australia     | Oceania   |  1952|  69.12000|     8691212|  10039.5956|
| Australia     | Oceania   |  1957|  70.33000|     9712569|  10949.6496|
| Australia     | Oceania   |  1962|  70.93000|    10794968|  12217.2269|
| Australia     | Oceania   |  1967|  71.10000|    11872264|  14526.1246|
| Australia     | Oceania   |  1972|  71.93000|    13177000|  16788.6295|
| Australia     | Oceania   |  1977|  73.49000|    14074100|  18334.1975|
| Australia     | Oceania   |  1982|  74.74000|    15184200|  19477.0093|
| Australia     | Oceania   |  1987|  76.32000|    16257249|  21888.8890|
| Australia     | Oceania   |  1992|  77.56000|    17481977|  23424.7668|
| Australia     | Oceania   |  1997|  78.83000|    18565243|  26997.9366|
| Australia     | Oceania   |  2002|  80.37000|    19546792|  30687.7547|
| Australia     | Oceania   |  2007|  81.23500|    20434176|  34435.3674|
| Brazil        | Americas  |  1952|  50.91700|    56602560|   2108.9444|
| Brazil        | Americas  |  1957|  53.28500|    65551171|   2487.3660|
| Brazil        | Americas  |  1962|  55.66500|    76039390|   3336.5858|
| Brazil        | Americas  |  1967|  57.63200|    88049823|   3429.8644|
| Brazil        | Americas  |  1972|  59.50400|   100840058|   4985.7115|
| Brazil        | Americas  |  1977|  61.48900|   114313951|   6660.1187|
| Brazil        | Americas  |  1982|  63.33600|   128962939|   7030.8359|
| Brazil        | Americas  |  1987|  65.20500|   142938076|   7807.0958|
| Brazil        | Americas  |  1992|  67.05700|   155975974|   6950.2830|
| Brazil        | Americas  |  1997|  69.38800|   168546719|   7957.9808|
| Brazil        | Americas  |  2002|  71.00600|   179914212|   8131.2128|
| Brazil        | Americas  |  2007|  72.39000|   190010647|   9065.8008|
| Canada        | Americas  |  1952|  68.75000|    14785584|  11367.1611|
| Canada        | Americas  |  1957|  69.96000|    17010154|  12489.9501|
| Canada        | Americas  |  1962|  71.30000|    18985849|  13462.4855|
| Canada        | Americas  |  1967|  72.13000|    20819767|  16076.5880|
| Canada        | Americas  |  1972|  72.88000|    22284500|  18970.5709|
| Canada        | Americas  |  1977|  74.21000|    23796400|  22090.8831|
| Canada        | Americas  |  1982|  75.76000|    25201900|  22898.7921|
| Canada        | Americas  |  1987|  76.86000|    26549700|  26626.5150|
| Canada        | Americas  |  1992|  77.95000|    28523502|  26342.8843|
| Canada        | Americas  |  1997|  78.61000|    30305843|  28954.9259|
| Canada        | Americas  |  2002|  79.77000|    31902268|  33328.9651|
| Canada        | Americas  |  2007|  80.65300|    33390141|  36319.2350|
| China         | Asia      |  1952|  44.00000|   556263527|    400.4486|
| China         | Asia      |  1957|  50.54896|   637408000|    575.9870|
| China         | Asia      |  1962|  44.50136|   665770000|    487.6740|
| China         | Asia      |  1967|  58.38112|   754550000|    612.7057|
| China         | Asia      |  1972|  63.11888|   862030000|    676.9001|
| China         | Asia      |  1977|  63.96736|   943455000|    741.2375|
| China         | Asia      |  1982|  65.52500|  1000281000|    962.4214|
| China         | Asia      |  1987|  67.27400|  1084035000|   1378.9040|
| China         | Asia      |  1992|  68.69000|  1164970000|   1655.7842|
| China         | Asia      |  1997|  70.42600|  1230075000|   2289.2341|
| China         | Asia      |  2002|  72.02800|  1280400000|   3119.2809|
| China         | Asia      |  2007|  72.96100|  1318683096|   4959.1149|
| Ethiopia      | Africa    |  1952|  34.07800|    20860941|    362.1463|
| Ethiopia      | Africa    |  1957|  36.66700|    22815614|    378.9042|
| Ethiopia      | Africa    |  1962|  40.05900|    25145372|    419.4564|
| Ethiopia      | Africa    |  1967|  42.11500|    27860297|    516.1186|
| Ethiopia      | Africa    |  1972|  43.51500|    30770372|    566.2439|
| Ethiopia      | Africa    |  1977|  44.51000|    34617799|    556.8084|
| Ethiopia      | Africa    |  1982|  44.91600|    38111756|    577.8607|
| Ethiopia      | Africa    |  1987|  46.68400|    42999530|    573.7413|
| Ethiopia      | Africa    |  1992|  48.09100|    52088559|    421.3535|
| Ethiopia      | Africa    |  1997|  49.40200|    59861301|    515.8894|
| Ethiopia      | Africa    |  2002|  50.72500|    67946797|    530.0535|
| Ethiopia      | Africa    |  2007|  52.94700|    76511887|    690.8056|
| France        | Europe    |  1952|  67.41000|    42459667|   7029.8093|
| France        | Europe    |  1957|  68.93000|    44310863|   8662.8349|
| France        | Europe    |  1962|  70.51000|    47124000|  10560.4855|
| France        | Europe    |  1967|  71.55000|    49569000|  12999.9177|
| France        | Europe    |  1972|  72.38000|    51732000|  16107.1917|
| France        | Europe    |  1977|  73.83000|    53165019|  18292.6351|
| France        | Europe    |  1982|  74.89000|    54433565|  20293.8975|
| France        | Europe    |  1987|  76.34000|    55630100|  22066.4421|
| France        | Europe    |  1992|  77.46000|    57374179|  24703.7961|
| France        | Europe    |  1997|  78.64000|    58623428|  25889.7849|
| France        | Europe    |  2002|  79.59000|    59925035|  28926.0323|
| France        | Europe    |  2007|  80.65700|    61083916|  30470.0167|
| Greece        | Europe    |  1952|  65.86000|     7733250|   3530.6901|
| Greece        | Europe    |  1957|  67.86000|     8096218|   4916.2999|
| Greece        | Europe    |  1962|  69.51000|     8448233|   6017.1907|
| Greece        | Europe    |  1967|  71.00000|     8716441|   8513.0970|
| Greece        | Europe    |  1972|  72.34000|     8888628|  12724.8296|
| Greece        | Europe    |  1977|  73.68000|     9308479|  14195.5243|
| Greece        | Europe    |  1982|  75.24000|     9786480|  15268.4209|
| Greece        | Europe    |  1987|  76.67000|     9974490|  16120.5284|
| Greece        | Europe    |  1992|  77.03000|    10325429|  17541.4963|
| Greece        | Europe    |  1997|  77.86900|    10502372|  18747.6981|
| Greece        | Europe    |  2002|  78.25600|    10603863|  22514.2548|
| Greece        | Europe    |  2007|  79.48300|    10706290|  27538.4119|
| Japan         | Asia      |  1952|  63.03000|    86459025|   3216.9563|
| Japan         | Asia      |  1957|  65.50000|    91563009|   4317.6944|
| Japan         | Asia      |  1962|  68.73000|    95831757|   6576.6495|
| Japan         | Asia      |  1967|  71.43000|   100825279|   9847.7886|
| Japan         | Asia      |  1972|  73.42000|   107188273|  14778.7864|
| Japan         | Asia      |  1977|  75.38000|   113872473|  16610.3770|
| Japan         | Asia      |  1982|  77.11000|   118454974|  19384.1057|
| Japan         | Asia      |  1987|  78.67000|   122091325|  22375.9419|
| Japan         | Asia      |  1992|  79.36000|   124329269|  26824.8951|
| Japan         | Asia      |  1997|  80.69000|   125956499|  28816.5850|
| Japan         | Asia      |  2002|  82.00000|   127065841|  28604.5919|
| Japan         | Asia      |  2007|  82.60300|   127467972|  31656.0681|
| Mexico        | Americas  |  1952|  50.78900|    30144317|   3478.1255|
| Mexico        | Americas  |  1957|  55.19000|    35015548|   4131.5466|
| Mexico        | Americas  |  1962|  58.29900|    41121485|   4581.6094|
| Mexico        | Americas  |  1967|  60.11000|    47995559|   5754.7339|
| Mexico        | Americas  |  1972|  62.36100|    55984294|   6809.4067|
| Mexico        | Americas  |  1977|  65.03200|    63759976|   7674.9291|
| Mexico        | Americas  |  1982|  67.40500|    71640904|   9611.1475|
| Mexico        | Americas  |  1987|  69.49800|    80122492|   8688.1560|
| Mexico        | Americas  |  1992|  71.45500|    88111030|   9472.3843|
| Mexico        | Americas  |  1997|  73.67000|    95895146|   9767.2975|
| Mexico        | Americas  |  2002|  74.90200|   102479927|  10742.4405|
| Mexico        | Americas  |  2007|  76.19500|   108700891|  11977.5750|
| New Zealand   | Oceania   |  1952|  69.39000|     1994794|  10556.5757|
| New Zealand   | Oceania   |  1957|  70.26000|     2229407|  12247.3953|
| New Zealand   | Oceania   |  1962|  71.24000|     2488550|  13175.6780|
| New Zealand   | Oceania   |  1967|  71.52000|     2728150|  14463.9189|
| New Zealand   | Oceania   |  1972|  71.89000|     2929100|  16046.0373|
| New Zealand   | Oceania   |  1977|  72.22000|     3164900|  16233.7177|
| New Zealand   | Oceania   |  1982|  73.84000|     3210650|  17632.4104|
| New Zealand   | Oceania   |  1987|  74.32000|     3317166|  19007.1913|
| New Zealand   | Oceania   |  1992|  76.33000|     3437674|  18363.3249|
| New Zealand   | Oceania   |  1997|  77.55000|     3676187|  21050.4138|
| New Zealand   | Oceania   |  2002|  79.11000|     3908037|  23189.8014|
| New Zealand   | Oceania   |  2007|  80.20400|     4115771|  25185.0091|
| Norway        | Europe    |  1952|  72.67000|     3327728|  10095.4217|
| Norway        | Europe    |  1957|  73.44000|     3491938|  11653.9730|
| Norway        | Europe    |  1962|  73.47000|     3638919|  13450.4015|
| Norway        | Europe    |  1967|  74.08000|     3786019|  16361.8765|
| Norway        | Europe    |  1972|  74.34000|     3933004|  18965.0555|
| Norway        | Europe    |  1977|  75.37000|     4043205|  23311.3494|
| Norway        | Europe    |  1982|  75.97000|     4114787|  26298.6353|
| Norway        | Europe    |  1987|  75.89000|     4186147|  31540.9748|
| Norway        | Europe    |  1992|  77.32000|     4286357|  33965.6611|
| Norway        | Europe    |  1997|  78.32000|     4405672|  41283.1643|
| Norway        | Europe    |  2002|  79.05000|     4535591|  44683.9753|
| Norway        | Europe    |  2007|  80.19600|     4627926|  49357.1902|
| Somalia       | Africa    |  1952|  32.97800|     2526994|   1135.7498|
| Somalia       | Africa    |  1957|  34.97700|     2780415|   1258.1474|
| Somalia       | Africa    |  1962|  36.98100|     3080153|   1369.4883|
| Somalia       | Africa    |  1967|  38.97700|     3428839|   1284.7332|
| Somalia       | Africa    |  1972|  40.97300|     3840161|   1254.5761|
| Somalia       | Africa    |  1977|  41.97400|     4353666|   1450.9925|
| Somalia       | Africa    |  1982|  42.95500|     5828892|   1176.8070|
| Somalia       | Africa    |  1987|  44.50100|     6921858|   1093.2450|
| Somalia       | Africa    |  1992|  39.65800|     6099799|    926.9603|
| Somalia       | Africa    |  1997|  43.79500|     6633514|    930.5964|
| Somalia       | Africa    |  2002|  45.93600|     7753310|    882.0818|
| Somalia       | Africa    |  2007|  48.15900|     9118773|    926.1411|
| Thailand      | Asia      |  1952|  50.84800|    21289402|    757.7974|
| Thailand      | Asia      |  1957|  53.63000|    25041917|    793.5774|
| Thailand      | Asia      |  1962|  56.06100|    29263397|   1002.1992|
| Thailand      | Asia      |  1967|  58.28500|    34024249|   1295.4607|
| Thailand      | Asia      |  1972|  60.40500|    39276153|   1524.3589|
| Thailand      | Asia      |  1977|  62.49400|    44148285|   1961.2246|
| Thailand      | Asia      |  1982|  64.59700|    48827160|   2393.2198|
| Thailand      | Asia      |  1987|  66.08400|    52910342|   2982.6538|
| Thailand      | Asia      |  1992|  67.29800|    56667095|   4616.8965|
| Thailand      | Asia      |  1997|  67.52100|    60216677|   5852.6255|
| Thailand      | Asia      |  2002|  68.56400|    62806748|   5913.1875|
| Thailand      | Asia      |  2007|  70.61600|    65068149|   7458.3963|
| Turkey        | Europe    |  1952|  43.58500|    22235677|   1969.1010|
| Turkey        | Europe    |  1957|  48.07900|    25670939|   2218.7543|
| Turkey        | Europe    |  1962|  52.09800|    29788695|   2322.8699|
| Turkey        | Europe    |  1967|  54.33600|    33411317|   2826.3564|
| Turkey        | Europe    |  1972|  57.00500|    37492953|   3450.6964|
| Turkey        | Europe    |  1977|  59.50700|    42404033|   4269.1223|
| Turkey        | Europe    |  1982|  61.03600|    47328791|   4241.3563|
| Turkey        | Europe    |  1987|  63.10800|    52881328|   5089.0437|
| Turkey        | Europe    |  1992|  66.14600|    58179144|   5678.3483|
| Turkey        | Europe    |  1997|  68.83500|    63047647|   6601.4299|
| Turkey        | Europe    |  2002|  70.84500|    67308928|   6508.0857|
| Turkey        | Europe    |  2007|  71.77700|    71158647|   8458.2764|
| United States | Americas  |  1952|  68.44000|   157553000|  13990.4821|
| United States | Americas  |  1957|  69.49000|   171984000|  14847.1271|
| United States | Americas  |  1962|  70.21000|   186538000|  16173.1459|
| United States | Americas  |  1967|  70.76000|   198712000|  19530.3656|
| United States | Americas  |  1972|  71.34000|   209896000|  21806.0359|
| United States | Americas  |  1977|  73.38000|   220239000|  24072.6321|
| United States | Americas  |  1982|  74.65000|   232187835|  25009.5591|
| United States | Americas  |  1987|  75.02000|   242803533|  29884.3504|
| United States | Americas  |  1992|  76.09000|   256894189|  32003.9322|
| United States | Americas  |  1997|  76.81000|   272911760|  35767.4330|
| United States | Americas  |  2002|  77.31000|   287675526|  39097.0995|
| United States | Americas  |  2007|  78.24200|   301139947|  42951.6531|
| Uruguay       | Americas  |  1952|  66.07100|     2252965|   5716.7667|
| Uruguay       | Americas  |  1957|  67.04400|     2424959|   6150.7730|
| Uruguay       | Americas  |  1962|  68.25300|     2598466|   5603.3577|
| Uruguay       | Americas  |  1967|  68.46800|     2748579|   5444.6196|
| Uruguay       | Americas  |  1972|  68.67300|     2829526|   5703.4089|
| Uruguay       | Americas  |  1977|  69.48100|     2873520|   6504.3397|
| Uruguay       | Americas  |  1982|  70.80500|     2953997|   6920.2231|
| Uruguay       | Americas  |  1987|  71.91800|     3045153|   7452.3990|
| Uruguay       | Americas  |  1992|  72.75200|     3149262|   8137.0048|
| Uruguay       | Americas  |  1997|  74.22300|     3262838|   9230.2407|
| Uruguay       | Americas  |  2002|  75.30700|     3363085|   7727.0020|
| Uruguay       | Americas  |  2007|  76.38400|     3447496|  10611.4630|
| Zimbabwe      | Africa    |  1952|  48.45100|     3080907|    406.8841|
| Zimbabwe      | Africa    |  1957|  50.46900|     3646340|    518.7643|
| Zimbabwe      | Africa    |  1962|  52.35800|     4277736|    527.2722|
| Zimbabwe      | Africa    |  1967|  53.99500|     4995432|    569.7951|
| Zimbabwe      | Africa    |  1972|  55.63500|     5861135|    799.3622|
| Zimbabwe      | Africa    |  1977|  57.67400|     6642107|    685.5877|
| Zimbabwe      | Africa    |  1982|  60.36300|     7636524|    788.8550|
| Zimbabwe      | Africa    |  1987|  62.35100|     9216418|    706.1573|
| Zimbabwe      | Africa    |  1992|  60.37700|    10704340|    693.4208|
| Zimbabwe      | Africa    |  1997|  46.80900|    11404948|    792.4500|
| Zimbabwe      | Africa    |  2002|  39.98900|    11926563|    672.0386|
| Zimbabwe      | Africa    |  2007|  43.48700|    12311143|    469.7093|

The result is similar to `inner_join()` but the capital column from countries\_18 is not added. Ie this function filters gapminder and only keeps countries that are present in the 2nd dataset `country` column but doesn't modify it otherwise.

``` r
joined_anti <- anti_join(gapminder, countries_18)
```

    ## Joining, by = "country"

    ## Warning: Column `country` joining factor and character vector, coercing
    ## into character vector

``` r
str(joined_anti)
```

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    1500 obs. of  6 variables:
    ##  $ country  : Factor w/ 142 levels "Afghanistan",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ continent: Factor w/ 5 levels "Africa","Americas",..: 3 3 3 3 3 3 3 3 3 3 ...
    ##  $ year     : int  1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
    ##  $ lifeExp  : num  28.8 30.3 32 34 36.1 ...
    ##  $ pop      : int  8425333 9240934 10267083 11537966 13079460 14880372 12881816 13867957 16317921 22227415 ...
    ##  $ gdpPercap: num  779 821 853 836 740 ...

``` r
levels(as.factor(joined_anti$country))
```

    ##   [1] "Afghanistan"              "Albania"                 
    ##   [3] "Algeria"                  "Angola"                  
    ##   [5] "Argentina"                "Australia"               
    ##   [7] "Austria"                  "Bahrain"                 
    ##   [9] "Bangladesh"               "Belgium"                 
    ##  [11] "Benin"                    "Bolivia"                 
    ##  [13] "Bosnia and Herzegovina"   "Botswana"                
    ##  [15] "Brazil"                   "Bulgaria"                
    ##  [17] "Burkina Faso"             "Burundi"                 
    ##  [19] "Cambodia"                 "Cameroon"                
    ##  [21] "Canada"                   "Central African Republic"
    ##  [23] "Chad"                     "Chile"                   
    ##  [25] "China"                    "Colombia"                
    ##  [27] "Comoros"                  "Congo, Dem. Rep."        
    ##  [29] "Congo, Rep."              "Costa Rica"              
    ##  [31] "Cote d'Ivoire"            "Croatia"                 
    ##  [33] "Cuba"                     "Czech Republic"          
    ##  [35] "Denmark"                  "Djibouti"                
    ##  [37] "Dominican Republic"       "Ecuador"                 
    ##  [39] "Egypt"                    "El Salvador"             
    ##  [41] "Equatorial Guinea"        "Eritrea"                 
    ##  [43] "Ethiopia"                 "Finland"                 
    ##  [45] "France"                   "Gabon"                   
    ##  [47] "Gambia"                   "Germany"                 
    ##  [49] "Ghana"                    "Greece"                  
    ##  [51] "Guatemala"                "Guinea"                  
    ##  [53] "Guinea-Bissau"            "Haiti"                   
    ##  [55] "Honduras"                 "Hong Kong, China"        
    ##  [57] "Hungary"                  "Iceland"                 
    ##  [59] "India"                    "Indonesia"               
    ##  [61] "Iran"                     "Iraq"                    
    ##  [63] "Ireland"                  "Israel"                  
    ##  [65] "Italy"                    "Jamaica"                 
    ##  [67] "Japan"                    "Jordan"                  
    ##  [69] "Kenya"                    "Korea, Dem. Rep."        
    ##  [71] "Korea, Rep."              "Kuwait"                  
    ##  [73] "Lebanon"                  "Lesotho"                 
    ##  [75] "Liberia"                  "Libya"                   
    ##  [77] "Madagascar"               "Malawi"                  
    ##  [79] "Malaysia"                 "Mali"                    
    ##  [81] "Mauritania"               "Mauritius"               
    ##  [83] "Mexico"                   "Mongolia"                
    ##  [85] "Montenegro"               "Morocco"                 
    ##  [87] "Mozambique"               "Myanmar"                 
    ##  [89] "Namibia"                  "Nepal"                   
    ##  [91] "Netherlands"              "New Zealand"             
    ##  [93] "Nicaragua"                "Niger"                   
    ##  [95] "Nigeria"                  "Norway"                  
    ##  [97] "Oman"                     "Pakistan"                
    ##  [99] "Panama"                   "Paraguay"                
    ## [101] "Peru"                     "Philippines"             
    ## [103] "Poland"                   "Portugal"                
    ## [105] "Puerto Rico"              "Reunion"                 
    ## [107] "Romania"                  "Rwanda"                  
    ## [109] "Sao Tome and Principe"    "Saudi Arabia"            
    ## [111] "Senegal"                  "Serbia"                  
    ## [113] "Sierra Leone"             "Singapore"               
    ## [115] "Slovak Republic"          "Slovenia"                
    ## [117] "Somalia"                  "South Africa"            
    ## [119] "Spain"                    "Sri Lanka"               
    ## [121] "Sudan"                    "Swaziland"               
    ## [123] "Sweden"                   "Switzerland"             
    ## [125] "Syria"                    "Taiwan"                  
    ## [127] "Tanzania"                 "Thailand"                
    ## [129] "Togo"                     "Trinidad and Tobago"     
    ## [131] "Tunisia"                  "Turkey"                  
    ## [133] "Uganda"                   "United Kingdom"          
    ## [135] "United States"            "Uruguay"                 
    ## [137] "Venezuela"                "Vietnam"                 
    ## [139] "West Bank and Gaza"       "Yemen, Rep."             
    ## [141] "Zambia"                   "Zimbabwe"

``` r
levels(as.factor(joined_anti$year))
```

    ##  [1] "1952" "1957" "1962" "1967" "1972" "1977" "1982" "1987" "1992" "1997"
    ## [11] "2002" "2007"

``` r
knitr::kable(joined_anti)
```

| country                  | continent |  year|  lifeExp|         pop|    gdpPercap|
|:-------------------------|:----------|-----:|--------:|-----------:|------------:|
| Afghanistan              | Asia      |  1952|   28.801|     8425333|     779.4453|
| Afghanistan              | Asia      |  1957|   30.332|     9240934|     820.8530|
| Afghanistan              | Asia      |  1962|   31.997|    10267083|     853.1007|
| Afghanistan              | Asia      |  1967|   34.020|    11537966|     836.1971|
| Afghanistan              | Asia      |  1972|   36.088|    13079460|     739.9811|
| Afghanistan              | Asia      |  1977|   38.438|    14880372|     786.1134|
| Afghanistan              | Asia      |  1982|   39.854|    12881816|     978.0114|
| Afghanistan              | Asia      |  1987|   40.822|    13867957|     852.3959|
| Afghanistan              | Asia      |  1992|   41.674|    16317921|     649.3414|
| Afghanistan              | Asia      |  1997|   41.763|    22227415|     635.3414|
| Afghanistan              | Asia      |  2002|   42.129|    25268405|     726.7341|
| Afghanistan              | Asia      |  2007|   43.828|    31889923|     974.5803|
| Albania                  | Europe    |  1952|   55.230|     1282697|    1601.0561|
| Albania                  | Europe    |  1957|   59.280|     1476505|    1942.2842|
| Albania                  | Europe    |  1962|   64.820|     1728137|    2312.8890|
| Albania                  | Europe    |  1967|   66.220|     1984060|    2760.1969|
| Albania                  | Europe    |  1972|   67.690|     2263554|    3313.4222|
| Albania                  | Europe    |  1977|   68.930|     2509048|    3533.0039|
| Albania                  | Europe    |  1982|   70.420|     2780097|    3630.8807|
| Albania                  | Europe    |  1987|   72.000|     3075321|    3738.9327|
| Albania                  | Europe    |  1992|   71.581|     3326498|    2497.4379|
| Albania                  | Europe    |  1997|   72.950|     3428038|    3193.0546|
| Albania                  | Europe    |  2002|   75.651|     3508512|    4604.2117|
| Albania                  | Europe    |  2007|   76.423|     3600523|    5937.0295|
| Algeria                  | Africa    |  1952|   43.077|     9279525|    2449.0082|
| Algeria                  | Africa    |  1957|   45.685|    10270856|    3013.9760|
| Algeria                  | Africa    |  1962|   48.303|    11000948|    2550.8169|
| Algeria                  | Africa    |  1967|   51.407|    12760499|    3246.9918|
| Algeria                  | Africa    |  1972|   54.518|    14760787|    4182.6638|
| Algeria                  | Africa    |  1977|   58.014|    17152804|    4910.4168|
| Algeria                  | Africa    |  1982|   61.368|    20033753|    5745.1602|
| Algeria                  | Africa    |  1987|   65.799|    23254956|    5681.3585|
| Algeria                  | Africa    |  1992|   67.744|    26298373|    5023.2166|
| Algeria                  | Africa    |  1997|   69.152|    29072015|    4797.2951|
| Algeria                  | Africa    |  2002|   70.994|    31287142|    5288.0404|
| Algeria                  | Africa    |  2007|   72.301|    33333216|    6223.3675|
| Angola                   | Africa    |  1952|   30.015|     4232095|    3520.6103|
| Angola                   | Africa    |  1957|   31.999|     4561361|    3827.9405|
| Angola                   | Africa    |  1962|   34.000|     4826015|    4269.2767|
| Angola                   | Africa    |  1967|   35.985|     5247469|    5522.7764|
| Angola                   | Africa    |  1972|   37.928|     5894858|    5473.2880|
| Angola                   | Africa    |  1977|   39.483|     6162675|    3008.6474|
| Angola                   | Africa    |  1982|   39.942|     7016384|    2756.9537|
| Angola                   | Africa    |  1987|   39.906|     7874230|    2430.2083|
| Angola                   | Africa    |  1992|   40.647|     8735988|    2627.8457|
| Angola                   | Africa    |  1997|   40.963|     9875024|    2277.1409|
| Angola                   | Africa    |  2002|   41.003|    10866106|    2773.2873|
| Angola                   | Africa    |  2007|   42.731|    12420476|    4797.2313|
| Argentina                | Americas  |  1952|   62.485|    17876956|    5911.3151|
| Argentina                | Americas  |  1957|   64.399|    19610538|    6856.8562|
| Argentina                | Americas  |  1962|   65.142|    21283783|    7133.1660|
| Argentina                | Americas  |  1967|   65.634|    22934225|    8052.9530|
| Argentina                | Americas  |  1972|   67.065|    24779799|    9443.0385|
| Argentina                | Americas  |  1977|   68.481|    26983828|   10079.0267|
| Argentina                | Americas  |  1982|   69.942|    29341374|    8997.8974|
| Argentina                | Americas  |  1987|   70.774|    31620918|    9139.6714|
| Argentina                | Americas  |  1992|   71.868|    33958947|    9308.4187|
| Argentina                | Americas  |  1997|   73.275|    36203463|   10967.2820|
| Argentina                | Americas  |  2002|   74.340|    38331121|    8797.6407|
| Argentina                | Americas  |  2007|   75.320|    40301927|   12779.3796|
| Austria                  | Europe    |  1952|   66.800|     6927772|    6137.0765|
| Austria                  | Europe    |  1957|   67.480|     6965860|    8842.5980|
| Austria                  | Europe    |  1962|   69.540|     7129864|   10750.7211|
| Austria                  | Europe    |  1967|   70.140|     7376998|   12834.6024|
| Austria                  | Europe    |  1972|   70.630|     7544201|   16661.6256|
| Austria                  | Europe    |  1977|   72.170|     7568430|   19749.4223|
| Austria                  | Europe    |  1982|   73.180|     7574613|   21597.0836|
| Austria                  | Europe    |  1987|   74.940|     7578903|   23687.8261|
| Austria                  | Europe    |  1992|   76.040|     7914969|   27042.0187|
| Austria                  | Europe    |  1997|   77.510|     8069876|   29095.9207|
| Austria                  | Europe    |  2002|   78.980|     8148312|   32417.6077|
| Austria                  | Europe    |  2007|   79.829|     8199783|   36126.4927|
| Bahrain                  | Asia      |  1952|   50.939|      120447|    9867.0848|
| Bahrain                  | Asia      |  1957|   53.832|      138655|   11635.7995|
| Bahrain                  | Asia      |  1962|   56.923|      171863|   12753.2751|
| Bahrain                  | Asia      |  1967|   59.923|      202182|   14804.6727|
| Bahrain                  | Asia      |  1972|   63.300|      230800|   18268.6584|
| Bahrain                  | Asia      |  1977|   65.593|      297410|   19340.1020|
| Bahrain                  | Asia      |  1982|   69.052|      377967|   19211.1473|
| Bahrain                  | Asia      |  1987|   70.750|      454612|   18524.0241|
| Bahrain                  | Asia      |  1992|   72.601|      529491|   19035.5792|
| Bahrain                  | Asia      |  1997|   73.925|      598561|   20292.0168|
| Bahrain                  | Asia      |  2002|   74.795|      656397|   23403.5593|
| Bahrain                  | Asia      |  2007|   75.635|      708573|   29796.0483|
| Bangladesh               | Asia      |  1952|   37.484|    46886859|     684.2442|
| Bangladesh               | Asia      |  1957|   39.348|    51365468|     661.6375|
| Bangladesh               | Asia      |  1962|   41.216|    56839289|     686.3416|
| Bangladesh               | Asia      |  1967|   43.453|    62821884|     721.1861|
| Bangladesh               | Asia      |  1972|   45.252|    70759295|     630.2336|
| Bangladesh               | Asia      |  1977|   46.923|    80428306|     659.8772|
| Bangladesh               | Asia      |  1982|   50.009|    93074406|     676.9819|
| Bangladesh               | Asia      |  1987|   52.819|   103764241|     751.9794|
| Bangladesh               | Asia      |  1992|   56.018|   113704579|     837.8102|
| Bangladesh               | Asia      |  1997|   59.412|   123315288|     972.7700|
| Bangladesh               | Asia      |  2002|   62.013|   135656790|    1136.3904|
| Bangladesh               | Asia      |  2007|   64.062|   150448339|    1391.2538|
| Belgium                  | Europe    |  1952|   68.000|     8730405|    8343.1051|
| Belgium                  | Europe    |  1957|   69.240|     8989111|    9714.9606|
| Belgium                  | Europe    |  1962|   70.250|     9218400|   10991.2068|
| Belgium                  | Europe    |  1967|   70.940|     9556500|   13149.0412|
| Belgium                  | Europe    |  1972|   71.440|     9709100|   16672.1436|
| Belgium                  | Europe    |  1977|   72.800|     9821800|   19117.9745|
| Belgium                  | Europe    |  1982|   73.930|     9856303|   20979.8459|
| Belgium                  | Europe    |  1987|   75.350|     9870200|   22525.5631|
| Belgium                  | Europe    |  1992|   76.460|    10045622|   25575.5707|
| Belgium                  | Europe    |  1997|   77.530|    10199787|   27561.1966|
| Belgium                  | Europe    |  2002|   78.320|    10311970|   30485.8838|
| Belgium                  | Europe    |  2007|   79.441|    10392226|   33692.6051|
| Benin                    | Africa    |  1952|   38.223|     1738315|    1062.7522|
| Benin                    | Africa    |  1957|   40.358|     1925173|     959.6011|
| Benin                    | Africa    |  1962|   42.618|     2151895|     949.4991|
| Benin                    | Africa    |  1967|   44.885|     2427334|    1035.8314|
| Benin                    | Africa    |  1972|   47.014|     2761407|    1085.7969|
| Benin                    | Africa    |  1977|   49.190|     3168267|    1029.1613|
| Benin                    | Africa    |  1982|   50.904|     3641603|    1277.8976|
| Benin                    | Africa    |  1987|   52.337|     4243788|    1225.8560|
| Benin                    | Africa    |  1992|   53.919|     4981671|    1191.2077|
| Benin                    | Africa    |  1997|   54.777|     6066080|    1232.9753|
| Benin                    | Africa    |  2002|   54.406|     7026113|    1372.8779|
| Benin                    | Africa    |  2007|   56.728|     8078314|    1441.2849|
| Bolivia                  | Americas  |  1952|   40.414|     2883315|    2677.3263|
| Bolivia                  | Americas  |  1957|   41.890|     3211738|    2127.6863|
| Bolivia                  | Americas  |  1962|   43.428|     3593918|    2180.9725|
| Bolivia                  | Americas  |  1967|   45.032|     4040665|    2586.8861|
| Bolivia                  | Americas  |  1972|   46.714|     4565872|    2980.3313|
| Bolivia                  | Americas  |  1977|   50.023|     5079716|    3548.0978|
| Bolivia                  | Americas  |  1982|   53.859|     5642224|    3156.5105|
| Bolivia                  | Americas  |  1987|   57.251|     6156369|    2753.6915|
| Bolivia                  | Americas  |  1992|   59.957|     6893451|    2961.6997|
| Bolivia                  | Americas  |  1997|   62.050|     7693188|    3326.1432|
| Bolivia                  | Americas  |  2002|   63.883|     8445134|    3413.2627|
| Bolivia                  | Americas  |  2007|   65.554|     9119152|    3822.1371|
| Bosnia and Herzegovina   | Europe    |  1952|   53.820|     2791000|     973.5332|
| Bosnia and Herzegovina   | Europe    |  1957|   58.450|     3076000|    1353.9892|
| Bosnia and Herzegovina   | Europe    |  1962|   61.930|     3349000|    1709.6837|
| Bosnia and Herzegovina   | Europe    |  1967|   64.790|     3585000|    2172.3524|
| Bosnia and Herzegovina   | Europe    |  1972|   67.450|     3819000|    2860.1698|
| Bosnia and Herzegovina   | Europe    |  1977|   69.860|     4086000|    3528.4813|
| Bosnia and Herzegovina   | Europe    |  1982|   70.690|     4172693|    4126.6132|
| Bosnia and Herzegovina   | Europe    |  1987|   71.140|     4338977|    4314.1148|
| Bosnia and Herzegovina   | Europe    |  1992|   72.178|     4256013|    2546.7814|
| Bosnia and Herzegovina   | Europe    |  1997|   73.244|     3607000|    4766.3559|
| Bosnia and Herzegovina   | Europe    |  2002|   74.090|     4165416|    6018.9752|
| Bosnia and Herzegovina   | Europe    |  2007|   74.852|     4552198|    7446.2988|
| Botswana                 | Africa    |  1952|   47.622|      442308|     851.2411|
| Botswana                 | Africa    |  1957|   49.618|      474639|     918.2325|
| Botswana                 | Africa    |  1962|   51.520|      512764|     983.6540|
| Botswana                 | Africa    |  1967|   53.298|      553541|    1214.7093|
| Botswana                 | Africa    |  1972|   56.024|      619351|    2263.6111|
| Botswana                 | Africa    |  1977|   59.319|      781472|    3214.8578|
| Botswana                 | Africa    |  1982|   61.484|      970347|    4551.1421|
| Botswana                 | Africa    |  1987|   63.622|     1151184|    6205.8839|
| Botswana                 | Africa    |  1992|   62.745|     1342614|    7954.1116|
| Botswana                 | Africa    |  1997|   52.556|     1536536|    8647.1423|
| Botswana                 | Africa    |  2002|   46.634|     1630347|   11003.6051|
| Botswana                 | Africa    |  2007|   50.728|     1639131|   12569.8518|
| Bulgaria                 | Europe    |  1952|   59.600|     7274900|    2444.2866|
| Bulgaria                 | Europe    |  1957|   66.610|     7651254|    3008.6707|
| Bulgaria                 | Europe    |  1962|   69.510|     8012946|    4254.3378|
| Bulgaria                 | Europe    |  1967|   70.420|     8310226|    5577.0028|
| Bulgaria                 | Europe    |  1972|   70.900|     8576200|    6597.4944|
| Bulgaria                 | Europe    |  1977|   70.810|     8797022|    7612.2404|
| Bulgaria                 | Europe    |  1982|   71.080|     8892098|    8224.1916|
| Bulgaria                 | Europe    |  1987|   71.340|     8971958|    8239.8548|
| Bulgaria                 | Europe    |  1992|   71.190|     8658506|    6302.6234|
| Bulgaria                 | Europe    |  1997|   70.320|     8066057|    5970.3888|
| Bulgaria                 | Europe    |  2002|   72.140|     7661799|    7696.7777|
| Bulgaria                 | Europe    |  2007|   73.005|     7322858|   10680.7928|
| Burkina Faso             | Africa    |  1952|   31.975|     4469979|     543.2552|
| Burkina Faso             | Africa    |  1957|   34.906|     4713416|     617.1835|
| Burkina Faso             | Africa    |  1962|   37.814|     4919632|     722.5120|
| Burkina Faso             | Africa    |  1967|   40.697|     5127935|     794.8266|
| Burkina Faso             | Africa    |  1972|   43.591|     5433886|     854.7360|
| Burkina Faso             | Africa    |  1977|   46.137|     5889574|     743.3870|
| Burkina Faso             | Africa    |  1982|   48.122|     6634596|     807.1986|
| Burkina Faso             | Africa    |  1987|   49.557|     7586551|     912.0631|
| Burkina Faso             | Africa    |  1992|   50.260|     8878303|     931.7528|
| Burkina Faso             | Africa    |  1997|   50.324|    10352843|     946.2950|
| Burkina Faso             | Africa    |  2002|   50.650|    12251209|    1037.6452|
| Burkina Faso             | Africa    |  2007|   52.295|    14326203|    1217.0330|
| Burundi                  | Africa    |  1952|   39.031|     2445618|     339.2965|
| Burundi                  | Africa    |  1957|   40.533|     2667518|     379.5646|
| Burundi                  | Africa    |  1962|   42.045|     2961915|     355.2032|
| Burundi                  | Africa    |  1967|   43.548|     3330989|     412.9775|
| Burundi                  | Africa    |  1972|   44.057|     3529983|     464.0995|
| Burundi                  | Africa    |  1977|   45.910|     3834415|     556.1033|
| Burundi                  | Africa    |  1982|   47.471|     4580410|     559.6032|
| Burundi                  | Africa    |  1987|   48.211|     5126023|     621.8188|
| Burundi                  | Africa    |  1992|   44.736|     5809236|     631.6999|
| Burundi                  | Africa    |  1997|   45.326|     6121610|     463.1151|
| Burundi                  | Africa    |  2002|   47.360|     7021078|     446.4035|
| Burundi                  | Africa    |  2007|   49.580|     8390505|     430.0707|
| Cambodia                 | Asia      |  1952|   39.417|     4693836|     368.4693|
| Cambodia                 | Asia      |  1957|   41.366|     5322536|     434.0383|
| Cambodia                 | Asia      |  1962|   43.415|     6083619|     496.9136|
| Cambodia                 | Asia      |  1967|   45.415|     6960067|     523.4323|
| Cambodia                 | Asia      |  1972|   40.317|     7450606|     421.6240|
| Cambodia                 | Asia      |  1977|   31.220|     6978607|     524.9722|
| Cambodia                 | Asia      |  1982|   50.957|     7272485|     624.4755|
| Cambodia                 | Asia      |  1987|   53.914|     8371791|     683.8956|
| Cambodia                 | Asia      |  1992|   55.803|    10150094|     682.3032|
| Cambodia                 | Asia      |  1997|   56.534|    11782962|     734.2852|
| Cambodia                 | Asia      |  2002|   56.752|    12926707|     896.2260|
| Cambodia                 | Asia      |  2007|   59.723|    14131858|    1713.7787|
| Cameroon                 | Africa    |  1952|   38.523|     5009067|    1172.6677|
| Cameroon                 | Africa    |  1957|   40.428|     5359923|    1313.0481|
| Cameroon                 | Africa    |  1962|   42.643|     5793633|    1399.6074|
| Cameroon                 | Africa    |  1967|   44.799|     6335506|    1508.4531|
| Cameroon                 | Africa    |  1972|   47.049|     7021028|    1684.1465|
| Cameroon                 | Africa    |  1977|   49.355|     7959865|    1783.4329|
| Cameroon                 | Africa    |  1982|   52.961|     9250831|    2367.9833|
| Cameroon                 | Africa    |  1987|   54.985|    10780667|    2602.6642|
| Cameroon                 | Africa    |  1992|   54.314|    12467171|    1793.1633|
| Cameroon                 | Africa    |  1997|   52.199|    14195809|    1694.3375|
| Cameroon                 | Africa    |  2002|   49.856|    15929988|    1934.0114|
| Cameroon                 | Africa    |  2007|   50.430|    17696293|    2042.0952|
| Central African Republic | Africa    |  1952|   35.463|     1291695|    1071.3107|
| Central African Republic | Africa    |  1957|   37.464|     1392284|    1190.8443|
| Central African Republic | Africa    |  1962|   39.475|     1523478|    1193.0688|
| Central African Republic | Africa    |  1967|   41.478|     1733638|    1136.0566|
| Central African Republic | Africa    |  1972|   43.457|     1927260|    1070.0133|
| Central African Republic | Africa    |  1977|   46.775|     2167533|    1109.3743|
| Central African Republic | Africa    |  1982|   48.295|     2476971|     956.7530|
| Central African Republic | Africa    |  1987|   50.485|     2840009|     844.8764|
| Central African Republic | Africa    |  1992|   49.396|     3265124|     747.9055|
| Central African Republic | Africa    |  1997|   46.066|     3696513|     740.5063|
| Central African Republic | Africa    |  2002|   43.308|     4048013|     738.6906|
| Central African Republic | Africa    |  2007|   44.741|     4369038|     706.0165|
| Chad                     | Africa    |  1952|   38.092|     2682462|    1178.6659|
| Chad                     | Africa    |  1957|   39.881|     2894855|    1308.4956|
| Chad                     | Africa    |  1962|   41.716|     3150417|    1389.8176|
| Chad                     | Africa    |  1967|   43.601|     3495967|    1196.8106|
| Chad                     | Africa    |  1972|   45.569|     3899068|    1104.1040|
| Chad                     | Africa    |  1977|   47.383|     4388260|    1133.9850|
| Chad                     | Africa    |  1982|   49.517|     4875118|     797.9081|
| Chad                     | Africa    |  1987|   51.051|     5498955|     952.3861|
| Chad                     | Africa    |  1992|   51.724|     6429417|    1058.0643|
| Chad                     | Africa    |  1997|   51.573|     7562011|    1004.9614|
| Chad                     | Africa    |  2002|   50.525|     8835739|    1156.1819|
| Chad                     | Africa    |  2007|   50.651|    10238807|    1704.0637|
| Chile                    | Americas  |  1952|   54.745|     6377619|    3939.9788|
| Chile                    | Americas  |  1957|   56.074|     7048426|    4315.6227|
| Chile                    | Americas  |  1962|   57.924|     7961258|    4519.0943|
| Chile                    | Americas  |  1967|   60.523|     8858908|    5106.6543|
| Chile                    | Americas  |  1972|   63.441|     9717524|    5494.0244|
| Chile                    | Americas  |  1977|   67.052|    10599793|    4756.7638|
| Chile                    | Americas  |  1982|   70.565|    11487112|    5095.6657|
| Chile                    | Americas  |  1987|   72.492|    12463354|    5547.0638|
| Chile                    | Americas  |  1992|   74.126|    13572994|    7596.1260|
| Chile                    | Americas  |  1997|   75.816|    14599929|   10118.0532|
| Chile                    | Americas  |  2002|   77.860|    15497046|   10778.7838|
| Chile                    | Americas  |  2007|   78.553|    16284741|   13171.6388|
| Colombia                 | Americas  |  1952|   50.643|    12350771|    2144.1151|
| Colombia                 | Americas  |  1957|   55.118|    14485993|    2323.8056|
| Colombia                 | Americas  |  1962|   57.863|    17009885|    2492.3511|
| Colombia                 | Americas  |  1967|   59.963|    19764027|    2678.7298|
| Colombia                 | Americas  |  1972|   61.623|    22542890|    3264.6600|
| Colombia                 | Americas  |  1977|   63.837|    25094412|    3815.8079|
| Colombia                 | Americas  |  1982|   66.653|    27764644|    4397.5757|
| Colombia                 | Americas  |  1987|   67.768|    30964245|    4903.2191|
| Colombia                 | Americas  |  1992|   68.421|    34202721|    5444.6486|
| Colombia                 | Americas  |  1997|   70.313|    37657830|    6117.3617|
| Colombia                 | Americas  |  2002|   71.682|    41008227|    5755.2600|
| Colombia                 | Americas  |  2007|   72.889|    44227550|    7006.5804|
| Comoros                  | Africa    |  1952|   40.715|      153936|    1102.9909|
| Comoros                  | Africa    |  1957|   42.460|      170928|    1211.1485|
| Comoros                  | Africa    |  1962|   44.467|      191689|    1406.6483|
| Comoros                  | Africa    |  1967|   46.472|      217378|    1876.0296|
| Comoros                  | Africa    |  1972|   48.944|      250027|    1937.5777|
| Comoros                  | Africa    |  1977|   50.939|      304739|    1172.6030|
| Comoros                  | Africa    |  1982|   52.933|      348643|    1267.1001|
| Comoros                  | Africa    |  1987|   54.926|      395114|    1315.9808|
| Comoros                  | Africa    |  1992|   57.939|      454429|    1246.9074|
| Comoros                  | Africa    |  1997|   60.660|      527982|    1173.6182|
| Comoros                  | Africa    |  2002|   62.974|      614382|    1075.8116|
| Comoros                  | Africa    |  2007|   65.152|      710960|     986.1479|
| Congo, Dem. Rep.         | Africa    |  1952|   39.143|    14100005|     780.5423|
| Congo, Dem. Rep.         | Africa    |  1957|   40.652|    15577932|     905.8602|
| Congo, Dem. Rep.         | Africa    |  1962|   42.122|    17486434|     896.3146|
| Congo, Dem. Rep.         | Africa    |  1967|   44.056|    19941073|     861.5932|
| Congo, Dem. Rep.         | Africa    |  1972|   45.989|    23007669|     904.8961|
| Congo, Dem. Rep.         | Africa    |  1977|   47.804|    26480870|     795.7573|
| Congo, Dem. Rep.         | Africa    |  1982|   47.784|    30646495|     673.7478|
| Congo, Dem. Rep.         | Africa    |  1987|   47.412|    35481645|     672.7748|
| Congo, Dem. Rep.         | Africa    |  1992|   45.548|    41672143|     457.7192|
| Congo, Dem. Rep.         | Africa    |  1997|   42.587|    47798986|     312.1884|
| Congo, Dem. Rep.         | Africa    |  2002|   44.966|    55379852|     241.1659|
| Congo, Dem. Rep.         | Africa    |  2007|   46.462|    64606759|     277.5519|
| Congo, Rep.              | Africa    |  1952|   42.111|      854885|    2125.6214|
| Congo, Rep.              | Africa    |  1957|   45.053|      940458|    2315.0566|
| Congo, Rep.              | Africa    |  1962|   48.435|     1047924|    2464.7832|
| Congo, Rep.              | Africa    |  1967|   52.040|     1179760|    2677.9396|
| Congo, Rep.              | Africa    |  1972|   54.907|     1340458|    3213.1527|
| Congo, Rep.              | Africa    |  1977|   55.625|     1536769|    3259.1790|
| Congo, Rep.              | Africa    |  1982|   56.695|     1774735|    4879.5075|
| Congo, Rep.              | Africa    |  1987|   57.470|     2064095|    4201.1949|
| Congo, Rep.              | Africa    |  1992|   56.433|     2409073|    4016.2395|
| Congo, Rep.              | Africa    |  1997|   52.962|     2800947|    3484.1644|
| Congo, Rep.              | Africa    |  2002|   52.970|     3328795|    3484.0620|
| Congo, Rep.              | Africa    |  2007|   55.322|     3800610|    3632.5578|
| Costa Rica               | Americas  |  1952|   57.206|      926317|    2627.0095|
| Costa Rica               | Americas  |  1957|   60.026|     1112300|    2990.0108|
| Costa Rica               | Americas  |  1962|   62.842|     1345187|    3460.9370|
| Costa Rica               | Americas  |  1967|   65.424|     1588717|    4161.7278|
| Costa Rica               | Americas  |  1972|   67.849|     1834796|    5118.1469|
| Costa Rica               | Americas  |  1977|   70.750|     2108457|    5926.8770|
| Costa Rica               | Americas  |  1982|   73.450|     2424367|    5262.7348|
| Costa Rica               | Americas  |  1987|   74.752|     2799811|    5629.9153|
| Costa Rica               | Americas  |  1992|   75.713|     3173216|    6160.4163|
| Costa Rica               | Americas  |  1997|   77.260|     3518107|    6677.0453|
| Costa Rica               | Americas  |  2002|   78.123|     3834934|    7723.4472|
| Costa Rica               | Americas  |  2007|   78.782|     4133884|    9645.0614|
| Cote d'Ivoire            | Africa    |  1952|   40.477|     2977019|    1388.5947|
| Cote d'Ivoire            | Africa    |  1957|   42.469|     3300000|    1500.8959|
| Cote d'Ivoire            | Africa    |  1962|   44.930|     3832408|    1728.8694|
| Cote d'Ivoire            | Africa    |  1967|   47.350|     4744870|    2052.0505|
| Cote d'Ivoire            | Africa    |  1972|   49.801|     6071696|    2378.2011|
| Cote d'Ivoire            | Africa    |  1977|   52.374|     7459574|    2517.7365|
| Cote d'Ivoire            | Africa    |  1982|   53.983|     9025951|    2602.7102|
| Cote d'Ivoire            | Africa    |  1987|   54.655|    10761098|    2156.9561|
| Cote d'Ivoire            | Africa    |  1992|   52.044|    12772596|    1648.0738|
| Cote d'Ivoire            | Africa    |  1997|   47.991|    14625967|    1786.2654|
| Cote d'Ivoire            | Africa    |  2002|   46.832|    16252726|    1648.8008|
| Cote d'Ivoire            | Africa    |  2007|   48.328|    18013409|    1544.7501|
| Croatia                  | Europe    |  1952|   61.210|     3882229|    3119.2365|
| Croatia                  | Europe    |  1957|   64.770|     3991242|    4338.2316|
| Croatia                  | Europe    |  1962|   67.130|     4076557|    5477.8900|
| Croatia                  | Europe    |  1967|   68.500|     4174366|    6960.2979|
| Croatia                  | Europe    |  1972|   69.610|     4225310|    9164.0901|
| Croatia                  | Europe    |  1977|   70.640|     4318673|   11305.3852|
| Croatia                  | Europe    |  1982|   70.460|     4413368|   13221.8218|
| Croatia                  | Europe    |  1987|   71.520|     4484310|   13822.5839|
| Croatia                  | Europe    |  1992|   72.527|     4494013|    8447.7949|
| Croatia                  | Europe    |  1997|   73.680|     4444595|    9875.6045|
| Croatia                  | Europe    |  2002|   74.876|     4481020|   11628.3890|
| Croatia                  | Europe    |  2007|   75.748|     4493312|   14619.2227|
| Cuba                     | Americas  |  1952|   59.421|     6007797|    5586.5388|
| Cuba                     | Americas  |  1957|   62.325|     6640752|    6092.1744|
| Cuba                     | Americas  |  1962|   65.246|     7254373|    5180.7559|
| Cuba                     | Americas  |  1967|   68.290|     8139332|    5690.2680|
| Cuba                     | Americas  |  1972|   70.723|     8831348|    5305.4453|
| Cuba                     | Americas  |  1977|   72.649|     9537988|    6380.4950|
| Cuba                     | Americas  |  1982|   73.717|     9789224|    7316.9181|
| Cuba                     | Americas  |  1987|   74.174|    10239839|    7532.9248|
| Cuba                     | Americas  |  1992|   74.414|    10723260|    5592.8440|
| Cuba                     | Americas  |  1997|   76.151|    10983007|    5431.9904|
| Cuba                     | Americas  |  2002|   77.158|    11226999|    6340.6467|
| Cuba                     | Americas  |  2007|   78.273|    11416987|    8948.1029|
| Czech Republic           | Europe    |  1952|   66.870|     9125183|    6876.1403|
| Czech Republic           | Europe    |  1957|   69.030|     9513758|    8256.3439|
| Czech Republic           | Europe    |  1962|   69.900|     9620282|   10136.8671|
| Czech Republic           | Europe    |  1967|   70.380|     9835109|   11399.4449|
| Czech Republic           | Europe    |  1972|   70.290|     9862158|   13108.4536|
| Czech Republic           | Europe    |  1977|   70.710|    10161915|   14800.1606|
| Czech Republic           | Europe    |  1982|   70.960|    10303704|   15377.2285|
| Czech Republic           | Europe    |  1987|   71.580|    10311597|   16310.4434|
| Czech Republic           | Europe    |  1992|   72.400|    10315702|   14297.0212|
| Czech Republic           | Europe    |  1997|   74.010|    10300707|   16048.5142|
| Czech Republic           | Europe    |  2002|   75.510|    10256295|   17596.2102|
| Czech Republic           | Europe    |  2007|   76.486|    10228744|   22833.3085|
| Denmark                  | Europe    |  1952|   70.780|     4334000|    9692.3852|
| Denmark                  | Europe    |  1957|   71.810|     4487831|   11099.6593|
| Denmark                  | Europe    |  1962|   72.350|     4646899|   13583.3135|
| Denmark                  | Europe    |  1967|   72.960|     4838800|   15937.2112|
| Denmark                  | Europe    |  1972|   73.470|     4991596|   18866.2072|
| Denmark                  | Europe    |  1977|   74.690|     5088419|   20422.9015|
| Denmark                  | Europe    |  1982|   74.630|     5117810|   21688.0405|
| Denmark                  | Europe    |  1987|   74.800|     5127024|   25116.1758|
| Denmark                  | Europe    |  1992|   75.330|     5171393|   26406.7399|
| Denmark                  | Europe    |  1997|   76.110|     5283663|   29804.3457|
| Denmark                  | Europe    |  2002|   77.180|     5374693|   32166.5001|
| Denmark                  | Europe    |  2007|   78.332|     5468120|   35278.4187|
| Djibouti                 | Africa    |  1952|   34.812|       63149|    2669.5295|
| Djibouti                 | Africa    |  1957|   37.328|       71851|    2864.9691|
| Djibouti                 | Africa    |  1962|   39.693|       89898|    3020.9893|
| Djibouti                 | Africa    |  1967|   42.074|      127617|    3020.0505|
| Djibouti                 | Africa    |  1972|   44.366|      178848|    3694.2124|
| Djibouti                 | Africa    |  1977|   46.519|      228694|    3081.7610|
| Djibouti                 | Africa    |  1982|   48.812|      305991|    2879.4681|
| Djibouti                 | Africa    |  1987|   50.040|      311025|    2880.1026|
| Djibouti                 | Africa    |  1992|   51.604|      384156|    2377.1562|
| Djibouti                 | Africa    |  1997|   53.157|      417908|    1895.0170|
| Djibouti                 | Africa    |  2002|   53.373|      447416|    1908.2609|
| Djibouti                 | Africa    |  2007|   54.791|      496374|    2082.4816|
| Dominican Republic       | Americas  |  1952|   45.928|     2491346|    1397.7171|
| Dominican Republic       | Americas  |  1957|   49.828|     2923186|    1544.4030|
| Dominican Republic       | Americas  |  1962|   53.459|     3453434|    1662.1374|
| Dominican Republic       | Americas  |  1967|   56.751|     4049146|    1653.7230|
| Dominican Republic       | Americas  |  1972|   59.631|     4671329|    2189.8745|
| Dominican Republic       | Americas  |  1977|   61.788|     5302800|    2681.9889|
| Dominican Republic       | Americas  |  1982|   63.727|     5968349|    2861.0924|
| Dominican Republic       | Americas  |  1987|   66.046|     6655297|    2899.8422|
| Dominican Republic       | Americas  |  1992|   68.457|     7351181|    3044.2142|
| Dominican Republic       | Americas  |  1997|   69.957|     7992357|    3614.1013|
| Dominican Republic       | Americas  |  2002|   70.847|     8650322|    4563.8082|
| Dominican Republic       | Americas  |  2007|   72.235|     9319622|    6025.3748|
| Ecuador                  | Americas  |  1952|   48.357|     3548753|    3522.1107|
| Ecuador                  | Americas  |  1957|   51.356|     4058385|    3780.5467|
| Ecuador                  | Americas  |  1962|   54.640|     4681707|    4086.1141|
| Ecuador                  | Americas  |  1967|   56.678|     5432424|    4579.0742|
| Ecuador                  | Americas  |  1972|   58.796|     6298651|    5280.9947|
| Ecuador                  | Americas  |  1977|   61.310|     7278866|    6679.6233|
| Ecuador                  | Americas  |  1982|   64.342|     8365850|    7213.7913|
| Ecuador                  | Americas  |  1987|   67.231|     9545158|    6481.7770|
| Ecuador                  | Americas  |  1992|   69.613|    10748394|    7103.7026|
| Ecuador                  | Americas  |  1997|   72.312|    11911819|    7429.4559|
| Ecuador                  | Americas  |  2002|   74.173|    12921234|    5773.0445|
| Ecuador                  | Americas  |  2007|   74.994|    13755680|    6873.2623|
| Egypt                    | Africa    |  1952|   41.893|    22223309|    1418.8224|
| Egypt                    | Africa    |  1957|   44.444|    25009741|    1458.9153|
| Egypt                    | Africa    |  1962|   46.992|    28173309|    1693.3359|
| Egypt                    | Africa    |  1967|   49.293|    31681188|    1814.8807|
| Egypt                    | Africa    |  1972|   51.137|    34807417|    2024.0081|
| Egypt                    | Africa    |  1977|   53.319|    38783863|    2785.4936|
| Egypt                    | Africa    |  1982|   56.006|    45681811|    3503.7296|
| Egypt                    | Africa    |  1987|   59.797|    52799062|    3885.4607|
| Egypt                    | Africa    |  1992|   63.674|    59402198|    3794.7552|
| Egypt                    | Africa    |  1997|   67.217|    66134291|    4173.1818|
| Egypt                    | Africa    |  2002|   69.806|    73312559|    4754.6044|
| Egypt                    | Africa    |  2007|   71.338|    80264543|    5581.1810|
| El Salvador              | Americas  |  1952|   45.262|     2042865|    3048.3029|
| El Salvador              | Americas  |  1957|   48.570|     2355805|    3421.5232|
| El Salvador              | Americas  |  1962|   52.307|     2747687|    3776.8036|
| El Salvador              | Americas  |  1967|   55.855|     3232927|    4358.5954|
| El Salvador              | Americas  |  1972|   58.207|     3790903|    4520.2460|
| El Salvador              | Americas  |  1977|   56.696|     4282586|    5138.9224|
| El Salvador              | Americas  |  1982|   56.604|     4474873|    4098.3442|
| El Salvador              | Americas  |  1987|   63.154|     4842194|    4140.4421|
| El Salvador              | Americas  |  1992|   66.798|     5274649|    4444.2317|
| El Salvador              | Americas  |  1997|   69.535|     5783439|    5154.8255|
| El Salvador              | Americas  |  2002|   70.734|     6353681|    5351.5687|
| El Salvador              | Americas  |  2007|   71.878|     6939688|    5728.3535|
| Equatorial Guinea        | Africa    |  1952|   34.482|      216964|     375.6431|
| Equatorial Guinea        | Africa    |  1957|   35.983|      232922|     426.0964|
| Equatorial Guinea        | Africa    |  1962|   37.485|      249220|     582.8420|
| Equatorial Guinea        | Africa    |  1967|   38.987|      259864|     915.5960|
| Equatorial Guinea        | Africa    |  1972|   40.516|      277603|     672.4123|
| Equatorial Guinea        | Africa    |  1977|   42.024|      192675|     958.5668|
| Equatorial Guinea        | Africa    |  1982|   43.662|      285483|     927.8253|
| Equatorial Guinea        | Africa    |  1987|   45.664|      341244|     966.8968|
| Equatorial Guinea        | Africa    |  1992|   47.545|      387838|    1132.0550|
| Equatorial Guinea        | Africa    |  1997|   48.245|      439971|    2814.4808|
| Equatorial Guinea        | Africa    |  2002|   49.348|      495627|    7703.4959|
| Equatorial Guinea        | Africa    |  2007|   51.579|      551201|   12154.0897|
| Eritrea                  | Africa    |  1952|   35.928|     1438760|     328.9406|
| Eritrea                  | Africa    |  1957|   38.047|     1542611|     344.1619|
| Eritrea                  | Africa    |  1962|   40.158|     1666618|     380.9958|
| Eritrea                  | Africa    |  1967|   42.189|     1820319|     468.7950|
| Eritrea                  | Africa    |  1972|   44.142|     2260187|     514.3242|
| Eritrea                  | Africa    |  1977|   44.535|     2512642|     505.7538|
| Eritrea                  | Africa    |  1982|   43.890|     2637297|     524.8758|
| Eritrea                  | Africa    |  1987|   46.453|     2915959|     521.1341|
| Eritrea                  | Africa    |  1992|   49.991|     3668440|     582.8585|
| Eritrea                  | Africa    |  1997|   53.378|     4058319|     913.4708|
| Eritrea                  | Africa    |  2002|   55.240|     4414865|     765.3500|
| Eritrea                  | Africa    |  2007|   58.040|     4906585|     641.3695|
| Finland                  | Europe    |  1952|   66.550|     4090500|    6424.5191|
| Finland                  | Europe    |  1957|   67.490|     4324000|    7545.4154|
| Finland                  | Europe    |  1962|   68.750|     4491443|    9371.8426|
| Finland                  | Europe    |  1967|   69.830|     4605744|   10921.6363|
| Finland                  | Europe    |  1972|   70.870|     4639657|   14358.8759|
| Finland                  | Europe    |  1977|   72.520|     4738902|   15605.4228|
| Finland                  | Europe    |  1982|   74.550|     4826933|   18533.1576|
| Finland                  | Europe    |  1987|   74.830|     4931729|   21141.0122|
| Finland                  | Europe    |  1992|   75.700|     5041039|   20647.1650|
| Finland                  | Europe    |  1997|   77.130|     5134406|   23723.9502|
| Finland                  | Europe    |  2002|   78.370|     5193039|   28204.5906|
| Finland                  | Europe    |  2007|   79.313|     5238460|   33207.0844|
| Gabon                    | Africa    |  1952|   37.003|      420702|    4293.4765|
| Gabon                    | Africa    |  1957|   38.999|      434904|    4976.1981|
| Gabon                    | Africa    |  1962|   40.489|      455661|    6631.4592|
| Gabon                    | Africa    |  1967|   44.598|      489004|    8358.7620|
| Gabon                    | Africa    |  1972|   48.690|      537977|   11401.9484|
| Gabon                    | Africa    |  1977|   52.790|      706367|   21745.5733|
| Gabon                    | Africa    |  1982|   56.564|      753874|   15113.3619|
| Gabon                    | Africa    |  1987|   60.190|      880397|   11864.4084|
| Gabon                    | Africa    |  1992|   61.366|      985739|   13522.1575|
| Gabon                    | Africa    |  1997|   60.461|     1126189|   14722.8419|
| Gabon                    | Africa    |  2002|   56.761|     1299304|   12521.7139|
| Gabon                    | Africa    |  2007|   56.735|     1454867|   13206.4845|
| Gambia                   | Africa    |  1952|   30.000|      284320|     485.2307|
| Gambia                   | Africa    |  1957|   32.065|      323150|     520.9267|
| Gambia                   | Africa    |  1962|   33.896|      374020|     599.6503|
| Gambia                   | Africa    |  1967|   35.857|      439593|     734.7829|
| Gambia                   | Africa    |  1972|   38.308|      517101|     756.0868|
| Gambia                   | Africa    |  1977|   41.842|      608274|     884.7553|
| Gambia                   | Africa    |  1982|   45.580|      715523|     835.8096|
| Gambia                   | Africa    |  1987|   49.265|      848406|     611.6589|
| Gambia                   | Africa    |  1992|   52.644|     1025384|     665.6244|
| Gambia                   | Africa    |  1997|   55.861|     1235767|     653.7302|
| Gambia                   | Africa    |  2002|   58.041|     1457766|     660.5856|
| Gambia                   | Africa    |  2007|   59.448|     1688359|     752.7497|
| Germany                  | Europe    |  1952|   67.500|    69145952|    7144.1144|
| Germany                  | Europe    |  1957|   69.100|    71019069|   10187.8267|
| Germany                  | Europe    |  1962|   70.300|    73739117|   12902.4629|
| Germany                  | Europe    |  1967|   70.800|    76368453|   14745.6256|
| Germany                  | Europe    |  1972|   71.000|    78717088|   18016.1803|
| Germany                  | Europe    |  1977|   72.500|    78160773|   20512.9212|
| Germany                  | Europe    |  1982|   73.800|    78335266|   22031.5327|
| Germany                  | Europe    |  1987|   74.847|    77718298|   24639.1857|
| Germany                  | Europe    |  1992|   76.070|    80597764|   26505.3032|
| Germany                  | Europe    |  1997|   77.340|    82011073|   27788.8842|
| Germany                  | Europe    |  2002|   78.670|    82350671|   30035.8020|
| Germany                  | Europe    |  2007|   79.406|    82400996|   32170.3744|
| Ghana                    | Africa    |  1952|   43.149|     5581001|     911.2989|
| Ghana                    | Africa    |  1957|   44.779|     6391288|    1043.5615|
| Ghana                    | Africa    |  1962|   46.452|     7355248|    1190.0411|
| Ghana                    | Africa    |  1967|   48.072|     8490213|    1125.6972|
| Ghana                    | Africa    |  1972|   49.875|     9354120|    1178.2237|
| Ghana                    | Africa    |  1977|   51.756|    10538093|     993.2240|
| Ghana                    | Africa    |  1982|   53.744|    11400338|     876.0326|
| Ghana                    | Africa    |  1987|   55.729|    14168101|     847.0061|
| Ghana                    | Africa    |  1992|   57.501|    16278738|     925.0602|
| Ghana                    | Africa    |  1997|   58.556|    18418288|    1005.2458|
| Ghana                    | Africa    |  2002|   58.453|    20550751|    1111.9846|
| Ghana                    | Africa    |  2007|   60.022|    22873338|    1327.6089|
| Guatemala                | Americas  |  1952|   42.023|     3146381|    2428.2378|
| Guatemala                | Americas  |  1957|   44.142|     3640876|    2617.1560|
| Guatemala                | Americas  |  1962|   46.954|     4208858|    2750.3644|
| Guatemala                | Americas  |  1967|   50.016|     4690773|    3242.5311|
| Guatemala                | Americas  |  1972|   53.738|     5149581|    4031.4083|
| Guatemala                | Americas  |  1977|   56.029|     5703430|    4879.9927|
| Guatemala                | Americas  |  1982|   58.137|     6395630|    4820.4948|
| Guatemala                | Americas  |  1987|   60.782|     7326406|    4246.4860|
| Guatemala                | Americas  |  1992|   63.373|     8486949|    4439.4508|
| Guatemala                | Americas  |  1997|   66.322|     9803875|    4684.3138|
| Guatemala                | Americas  |  2002|   68.978|    11178650|    4858.3475|
| Guatemala                | Americas  |  2007|   70.259|    12572928|    5186.0500|
| Guinea                   | Africa    |  1952|   33.609|     2664249|     510.1965|
| Guinea                   | Africa    |  1957|   34.558|     2876726|     576.2670|
| Guinea                   | Africa    |  1962|   35.753|     3140003|     686.3737|
| Guinea                   | Africa    |  1967|   37.197|     3451418|     708.7595|
| Guinea                   | Africa    |  1972|   38.842|     3811387|     741.6662|
| Guinea                   | Africa    |  1977|   40.762|     4227026|     874.6859|
| Guinea                   | Africa    |  1982|   42.891|     4710497|     857.2504|
| Guinea                   | Africa    |  1987|   45.552|     5650262|     805.5725|
| Guinea                   | Africa    |  1992|   48.576|     6990574|     794.3484|
| Guinea                   | Africa    |  1997|   51.455|     8048834|     869.4498|
| Guinea                   | Africa    |  2002|   53.676|     8807818|     945.5836|
| Guinea                   | Africa    |  2007|   56.007|     9947814|     942.6542|
| Guinea-Bissau            | Africa    |  1952|   32.500|      580653|     299.8503|
| Guinea-Bissau            | Africa    |  1957|   33.489|      601095|     431.7905|
| Guinea-Bissau            | Africa    |  1962|   34.488|      627820|     522.0344|
| Guinea-Bissau            | Africa    |  1967|   35.492|      601287|     715.5806|
| Guinea-Bissau            | Africa    |  1972|   36.486|      625361|     820.2246|
| Guinea-Bissau            | Africa    |  1977|   37.465|      745228|     764.7260|
| Guinea-Bissau            | Africa    |  1982|   39.327|      825987|     838.1240|
| Guinea-Bissau            | Africa    |  1987|   41.245|      927524|     736.4154|
| Guinea-Bissau            | Africa    |  1992|   43.266|     1050938|     745.5399|
| Guinea-Bissau            | Africa    |  1997|   44.873|     1193708|     796.6645|
| Guinea-Bissau            | Africa    |  2002|   45.504|     1332459|     575.7047|
| Guinea-Bissau            | Africa    |  2007|   46.388|     1472041|     579.2317|
| Haiti                    | Americas  |  1952|   37.579|     3201488|    1840.3669|
| Haiti                    | Americas  |  1957|   40.696|     3507701|    1726.8879|
| Haiti                    | Americas  |  1962|   43.590|     3880130|    1796.5890|
| Haiti                    | Americas  |  1967|   46.243|     4318137|    1452.0577|
| Haiti                    | Americas  |  1972|   48.042|     4698301|    1654.4569|
| Haiti                    | Americas  |  1977|   49.923|     4908554|    1874.2989|
| Haiti                    | Americas  |  1982|   51.461|     5198399|    2011.1595|
| Haiti                    | Americas  |  1987|   53.636|     5756203|    1823.0160|
| Haiti                    | Americas  |  1992|   55.089|     6326682|    1456.3095|
| Haiti                    | Americas  |  1997|   56.671|     6913545|    1341.7269|
| Haiti                    | Americas  |  2002|   58.137|     7607651|    1270.3649|
| Haiti                    | Americas  |  2007|   60.916|     8502814|    1201.6372|
| Honduras                 | Americas  |  1952|   41.912|     1517453|    2194.9262|
| Honduras                 | Americas  |  1957|   44.665|     1770390|    2220.4877|
| Honduras                 | Americas  |  1962|   48.041|     2090162|    2291.1568|
| Honduras                 | Americas  |  1967|   50.924|     2500689|    2538.2694|
| Honduras                 | Americas  |  1972|   53.884|     2965146|    2529.8423|
| Honduras                 | Americas  |  1977|   57.402|     3055235|    3203.2081|
| Honduras                 | Americas  |  1982|   60.909|     3669448|    3121.7608|
| Honduras                 | Americas  |  1987|   64.492|     4372203|    3023.0967|
| Honduras                 | Americas  |  1992|   66.399|     5077347|    3081.6946|
| Honduras                 | Americas  |  1997|   67.659|     5867957|    3160.4549|
| Honduras                 | Americas  |  2002|   68.565|     6677328|    3099.7287|
| Honduras                 | Americas  |  2007|   70.198|     7483763|    3548.3308|
| Hong Kong, China         | Asia      |  1952|   60.960|     2125900|    3054.4212|
| Hong Kong, China         | Asia      |  1957|   64.750|     2736300|    3629.0765|
| Hong Kong, China         | Asia      |  1962|   67.650|     3305200|    4692.6483|
| Hong Kong, China         | Asia      |  1967|   70.000|     3722800|    6197.9628|
| Hong Kong, China         | Asia      |  1972|   72.000|     4115700|    8315.9281|
| Hong Kong, China         | Asia      |  1977|   73.600|     4583700|   11186.1413|
| Hong Kong, China         | Asia      |  1982|   75.450|     5264500|   14560.5305|
| Hong Kong, China         | Asia      |  1987|   76.200|     5584510|   20038.4727|
| Hong Kong, China         | Asia      |  1992|   77.601|     5829696|   24757.6030|
| Hong Kong, China         | Asia      |  1997|   80.000|     6495918|   28377.6322|
| Hong Kong, China         | Asia      |  2002|   81.495|     6762476|   30209.0152|
| Hong Kong, China         | Asia      |  2007|   82.208|     6980412|   39724.9787|
| Hungary                  | Europe    |  1952|   64.030|     9504000|    5263.6738|
| Hungary                  | Europe    |  1957|   66.410|     9839000|    6040.1800|
| Hungary                  | Europe    |  1962|   67.960|    10063000|    7550.3599|
| Hungary                  | Europe    |  1967|   69.500|    10223422|    9326.6447|
| Hungary                  | Europe    |  1972|   69.760|    10394091|   10168.6561|
| Hungary                  | Europe    |  1977|   69.950|    10637171|   11674.8374|
| Hungary                  | Europe    |  1982|   69.390|    10705535|   12545.9907|
| Hungary                  | Europe    |  1987|   69.580|    10612740|   12986.4800|
| Hungary                  | Europe    |  1992|   69.170|    10348684|   10535.6285|
| Hungary                  | Europe    |  1997|   71.040|    10244684|   11712.7768|
| Hungary                  | Europe    |  2002|   72.590|    10083313|   14843.9356|
| Hungary                  | Europe    |  2007|   73.338|     9956108|   18008.9444|
| Iceland                  | Europe    |  1952|   72.490|      147962|    7267.6884|
| Iceland                  | Europe    |  1957|   73.470|      165110|    9244.0014|
| Iceland                  | Europe    |  1962|   73.680|      182053|   10350.1591|
| Iceland                  | Europe    |  1967|   73.730|      198676|   13319.8957|
| Iceland                  | Europe    |  1972|   74.460|      209275|   15798.0636|
| Iceland                  | Europe    |  1977|   76.110|      221823|   19654.9625|
| Iceland                  | Europe    |  1982|   76.990|      233997|   23269.6075|
| Iceland                  | Europe    |  1987|   77.230|      244676|   26923.2063|
| Iceland                  | Europe    |  1992|   78.770|      259012|   25144.3920|
| Iceland                  | Europe    |  1997|   78.950|      271192|   28061.0997|
| Iceland                  | Europe    |  2002|   80.500|      288030|   31163.2020|
| Iceland                  | Europe    |  2007|   81.757|      301931|   36180.7892|
| India                    | Asia      |  1952|   37.373|   372000000|     546.5657|
| India                    | Asia      |  1957|   40.249|   409000000|     590.0620|
| India                    | Asia      |  1962|   43.605|   454000000|     658.3472|
| India                    | Asia      |  1967|   47.193|   506000000|     700.7706|
| India                    | Asia      |  1972|   50.651|   567000000|     724.0325|
| India                    | Asia      |  1977|   54.208|   634000000|     813.3373|
| India                    | Asia      |  1982|   56.596|   708000000|     855.7235|
| India                    | Asia      |  1987|   58.553|   788000000|     976.5127|
| India                    | Asia      |  1992|   60.223|   872000000|    1164.4068|
| India                    | Asia      |  1997|   61.765|   959000000|    1458.8174|
| India                    | Asia      |  2002|   62.879|  1034172547|    1746.7695|
| India                    | Asia      |  2007|   64.698|  1110396331|    2452.2104|
| Indonesia                | Asia      |  1952|   37.468|    82052000|     749.6817|
| Indonesia                | Asia      |  1957|   39.918|    90124000|     858.9003|
| Indonesia                | Asia      |  1962|   42.518|    99028000|     849.2898|
| Indonesia                | Asia      |  1967|   45.964|   109343000|     762.4318|
| Indonesia                | Asia      |  1972|   49.203|   121282000|    1111.1079|
| Indonesia                | Asia      |  1977|   52.702|   136725000|    1382.7021|
| Indonesia                | Asia      |  1982|   56.159|   153343000|    1516.8730|
| Indonesia                | Asia      |  1987|   60.137|   169276000|    1748.3570|
| Indonesia                | Asia      |  1992|   62.681|   184816000|    2383.1409|
| Indonesia                | Asia      |  1997|   66.041|   199278000|    3119.3356|
| Indonesia                | Asia      |  2002|   68.588|   211060000|    2873.9129|
| Indonesia                | Asia      |  2007|   70.650|   223547000|    3540.6516|
| Iran                     | Asia      |  1952|   44.869|    17272000|    3035.3260|
| Iran                     | Asia      |  1957|   47.181|    19792000|    3290.2576|
| Iran                     | Asia      |  1962|   49.325|    22874000|    4187.3298|
| Iran                     | Asia      |  1967|   52.469|    26538000|    5906.7318|
| Iran                     | Asia      |  1972|   55.234|    30614000|    9613.8186|
| Iran                     | Asia      |  1977|   57.702|    35480679|   11888.5951|
| Iran                     | Asia      |  1982|   59.620|    43072751|    7608.3346|
| Iran                     | Asia      |  1987|   63.040|    51889696|    6642.8814|
| Iran                     | Asia      |  1992|   65.742|    60397973|    7235.6532|
| Iran                     | Asia      |  1997|   68.042|    63327987|    8263.5903|
| Iran                     | Asia      |  2002|   69.451|    66907826|    9240.7620|
| Iran                     | Asia      |  2007|   70.964|    69453570|   11605.7145|
| Iraq                     | Asia      |  1952|   45.320|     5441766|    4129.7661|
| Iraq                     | Asia      |  1957|   48.437|     6248643|    6229.3336|
| Iraq                     | Asia      |  1962|   51.457|     7240260|    8341.7378|
| Iraq                     | Asia      |  1967|   54.459|     8519282|    8931.4598|
| Iraq                     | Asia      |  1972|   56.950|    10061506|    9576.0376|
| Iraq                     | Asia      |  1977|   60.413|    11882916|   14688.2351|
| Iraq                     | Asia      |  1982|   62.038|    14173318|   14517.9071|
| Iraq                     | Asia      |  1987|   65.044|    16543189|   11643.5727|
| Iraq                     | Asia      |  1992|   59.461|    17861905|    3745.6407|
| Iraq                     | Asia      |  1997|   58.811|    20775703|    3076.2398|
| Iraq                     | Asia      |  2002|   57.046|    24001816|    4390.7173|
| Iraq                     | Asia      |  2007|   59.545|    27499638|    4471.0619|
| Ireland                  | Europe    |  1952|   66.910|     2952156|    5210.2803|
| Ireland                  | Europe    |  1957|   68.900|     2878220|    5599.0779|
| Ireland                  | Europe    |  1962|   70.290|     2830000|    6631.5973|
| Ireland                  | Europe    |  1967|   71.080|     2900100|    7655.5690|
| Ireland                  | Europe    |  1972|   71.280|     3024400|    9530.7729|
| Ireland                  | Europe    |  1977|   72.030|     3271900|   11150.9811|
| Ireland                  | Europe    |  1982|   73.100|     3480000|   12618.3214|
| Ireland                  | Europe    |  1987|   74.360|     3539900|   13872.8665|
| Ireland                  | Europe    |  1992|   75.467|     3557761|   17558.8155|
| Ireland                  | Europe    |  1997|   76.122|     3667233|   24521.9471|
| Ireland                  | Europe    |  2002|   77.783|     3879155|   34077.0494|
| Ireland                  | Europe    |  2007|   78.885|     4109086|   40675.9964|
| Israel                   | Asia      |  1952|   65.390|     1620914|    4086.5221|
| Israel                   | Asia      |  1957|   67.840|     1944401|    5385.2785|
| Israel                   | Asia      |  1962|   69.390|     2310904|    7105.6307|
| Israel                   | Asia      |  1967|   70.750|     2693585|    8393.7414|
| Israel                   | Asia      |  1972|   71.630|     3095893|   12786.9322|
| Israel                   | Asia      |  1977|   73.060|     3495918|   13306.6192|
| Israel                   | Asia      |  1982|   74.450|     3858421|   15367.0292|
| Israel                   | Asia      |  1987|   75.600|     4203148|   17122.4799|
| Israel                   | Asia      |  1992|   76.930|     4936550|   18051.5225|
| Israel                   | Asia      |  1997|   78.269|     5531387|   20896.6092|
| Israel                   | Asia      |  2002|   79.696|     6029529|   21905.5951|
| Israel                   | Asia      |  2007|   80.745|     6426679|   25523.2771|
| Italy                    | Europe    |  1952|   65.940|    47666000|    4931.4042|
| Italy                    | Europe    |  1957|   67.810|    49182000|    6248.6562|
| Italy                    | Europe    |  1962|   69.240|    50843200|    8243.5823|
| Italy                    | Europe    |  1967|   71.060|    52667100|   10022.4013|
| Italy                    | Europe    |  1972|   72.190|    54365564|   12269.2738|
| Italy                    | Europe    |  1977|   73.480|    56059245|   14255.9847|
| Italy                    | Europe    |  1982|   74.980|    56535636|   16537.4835|
| Italy                    | Europe    |  1987|   76.420|    56729703|   19207.2348|
| Italy                    | Europe    |  1992|   77.440|    56840847|   22013.6449|
| Italy                    | Europe    |  1997|   78.820|    57479469|   24675.0245|
| Italy                    | Europe    |  2002|   80.240|    57926999|   27968.0982|
| Italy                    | Europe    |  2007|   80.546|    58147733|   28569.7197|
| Jamaica                  | Americas  |  1952|   58.530|     1426095|    2898.5309|
| Jamaica                  | Americas  |  1957|   62.610|     1535090|    4756.5258|
| Jamaica                  | Americas  |  1962|   65.610|     1665128|    5246.1075|
| Jamaica                  | Americas  |  1967|   67.510|     1861096|    6124.7035|
| Jamaica                  | Americas  |  1972|   69.000|     1997616|    7433.8893|
| Jamaica                  | Americas  |  1977|   70.110|     2156814|    6650.1956|
| Jamaica                  | Americas  |  1982|   71.210|     2298309|    6068.0513|
| Jamaica                  | Americas  |  1987|   71.770|     2326606|    6351.2375|
| Jamaica                  | Americas  |  1992|   71.766|     2378618|    7404.9237|
| Jamaica                  | Americas  |  1997|   72.262|     2531311|    7121.9247|
| Jamaica                  | Americas  |  2002|   72.047|     2664659|    6994.7749|
| Jamaica                  | Americas  |  2007|   72.567|     2780132|    7320.8803|
| Jordan                   | Asia      |  1952|   43.158|      607914|    1546.9078|
| Jordan                   | Asia      |  1957|   45.669|      746559|    1886.0806|
| Jordan                   | Asia      |  1962|   48.126|      933559|    2348.0092|
| Jordan                   | Asia      |  1967|   51.629|     1255058|    2741.7963|
| Jordan                   | Asia      |  1972|   56.528|     1613551|    2110.8563|
| Jordan                   | Asia      |  1977|   61.134|     1937652|    2852.3516|
| Jordan                   | Asia      |  1982|   63.739|     2347031|    4161.4160|
| Jordan                   | Asia      |  1987|   65.869|     2820042|    4448.6799|
| Jordan                   | Asia      |  1992|   68.015|     3867409|    3431.5936|
| Jordan                   | Asia      |  1997|   69.772|     4526235|    3645.3796|
| Jordan                   | Asia      |  2002|   71.263|     5307470|    3844.9172|
| Jordan                   | Asia      |  2007|   72.535|     6053193|    4519.4612|
| Kenya                    | Africa    |  1952|   42.270|     6464046|     853.5409|
| Kenya                    | Africa    |  1957|   44.686|     7454779|     944.4383|
| Kenya                    | Africa    |  1962|   47.949|     8678557|     896.9664|
| Kenya                    | Africa    |  1967|   50.654|    10191512|    1056.7365|
| Kenya                    | Africa    |  1972|   53.559|    12044785|    1222.3600|
| Kenya                    | Africa    |  1977|   56.155|    14500404|    1267.6132|
| Kenya                    | Africa    |  1982|   58.766|    17661452|    1348.2258|
| Kenya                    | Africa    |  1987|   59.339|    21198082|    1361.9369|
| Kenya                    | Africa    |  1992|   59.285|    25020539|    1341.9217|
| Kenya                    | Africa    |  1997|   54.407|    28263827|    1360.4850|
| Kenya                    | Africa    |  2002|   50.992|    31386842|    1287.5147|
| Kenya                    | Africa    |  2007|   54.110|    35610177|    1463.2493|
| Korea, Dem. Rep.         | Asia      |  1952|   50.056|     8865488|    1088.2778|
| Korea, Dem. Rep.         | Asia      |  1957|   54.081|     9411381|    1571.1347|
| Korea, Dem. Rep.         | Asia      |  1962|   56.656|    10917494|    1621.6936|
| Korea, Dem. Rep.         | Asia      |  1967|   59.942|    12617009|    2143.5406|
| Korea, Dem. Rep.         | Asia      |  1972|   63.983|    14781241|    3701.6215|
| Korea, Dem. Rep.         | Asia      |  1977|   67.159|    16325320|    4106.3012|
| Korea, Dem. Rep.         | Asia      |  1982|   69.100|    17647518|    4106.5253|
| Korea, Dem. Rep.         | Asia      |  1987|   70.647|    19067554|    4106.4923|
| Korea, Dem. Rep.         | Asia      |  1992|   69.978|    20711375|    3726.0635|
| Korea, Dem. Rep.         | Asia      |  1997|   67.727|    21585105|    1690.7568|
| Korea, Dem. Rep.         | Asia      |  2002|   66.662|    22215365|    1646.7582|
| Korea, Dem. Rep.         | Asia      |  2007|   67.297|    23301725|    1593.0655|
| Korea, Rep.              | Asia      |  1952|   47.453|    20947571|    1030.5922|
| Korea, Rep.              | Asia      |  1957|   52.681|    22611552|    1487.5935|
| Korea, Rep.              | Asia      |  1962|   55.292|    26420307|    1536.3444|
| Korea, Rep.              | Asia      |  1967|   57.716|    30131000|    2029.2281|
| Korea, Rep.              | Asia      |  1972|   62.612|    33505000|    3030.8767|
| Korea, Rep.              | Asia      |  1977|   64.766|    36436000|    4657.2210|
| Korea, Rep.              | Asia      |  1982|   67.123|    39326000|    5622.9425|
| Korea, Rep.              | Asia      |  1987|   69.810|    41622000|    8533.0888|
| Korea, Rep.              | Asia      |  1992|   72.244|    43805450|   12104.2787|
| Korea, Rep.              | Asia      |  1997|   74.647|    46173816|   15993.5280|
| Korea, Rep.              | Asia      |  2002|   77.045|    47969150|   19233.9882|
| Korea, Rep.              | Asia      |  2007|   78.623|    49044790|   23348.1397|
| Kuwait                   | Asia      |  1952|   55.565|      160000|  108382.3529|
| Kuwait                   | Asia      |  1957|   58.033|      212846|  113523.1329|
| Kuwait                   | Asia      |  1962|   60.470|      358266|   95458.1118|
| Kuwait                   | Asia      |  1967|   64.624|      575003|   80894.8833|
| Kuwait                   | Asia      |  1972|   67.712|      841934|  109347.8670|
| Kuwait                   | Asia      |  1977|   69.343|     1140357|   59265.4771|
| Kuwait                   | Asia      |  1982|   71.309|     1497494|   31354.0357|
| Kuwait                   | Asia      |  1987|   74.174|     1891487|   28118.4300|
| Kuwait                   | Asia      |  1992|   75.190|     1418095|   34932.9196|
| Kuwait                   | Asia      |  1997|   76.156|     1765345|   40300.6200|
| Kuwait                   | Asia      |  2002|   76.904|     2111561|   35110.1057|
| Kuwait                   | Asia      |  2007|   77.588|     2505559|   47306.9898|
| Lebanon                  | Asia      |  1952|   55.928|     1439529|    4834.8041|
| Lebanon                  | Asia      |  1957|   59.489|     1647412|    6089.7869|
| Lebanon                  | Asia      |  1962|   62.094|     1886848|    5714.5606|
| Lebanon                  | Asia      |  1967|   63.870|     2186894|    6006.9830|
| Lebanon                  | Asia      |  1972|   65.421|     2680018|    7486.3843|
| Lebanon                  | Asia      |  1977|   66.099|     3115787|    8659.6968|
| Lebanon                  | Asia      |  1982|   66.983|     3086876|    7640.5195|
| Lebanon                  | Asia      |  1987|   67.926|     3089353|    5377.0913|
| Lebanon                  | Asia      |  1992|   69.292|     3219994|    6890.8069|
| Lebanon                  | Asia      |  1997|   70.265|     3430388|    8754.9639|
| Lebanon                  | Asia      |  2002|   71.028|     3677780|    9313.9388|
| Lebanon                  | Asia      |  2007|   71.993|     3921278|   10461.0587|
| Lesotho                  | Africa    |  1952|   42.138|      748747|     298.8462|
| Lesotho                  | Africa    |  1957|   45.047|      813338|     335.9971|
| Lesotho                  | Africa    |  1962|   47.747|      893143|     411.8006|
| Lesotho                  | Africa    |  1967|   48.492|      996380|     498.6390|
| Lesotho                  | Africa    |  1972|   49.767|     1116779|     496.5816|
| Lesotho                  | Africa    |  1977|   52.208|     1251524|     745.3695|
| Lesotho                  | Africa    |  1982|   55.078|     1411807|     797.2631|
| Lesotho                  | Africa    |  1987|   57.180|     1599200|     773.9932|
| Lesotho                  | Africa    |  1992|   59.685|     1803195|     977.4863|
| Lesotho                  | Africa    |  1997|   55.558|     1982823|    1186.1480|
| Lesotho                  | Africa    |  2002|   44.593|     2046772|    1275.1846|
| Lesotho                  | Africa    |  2007|   42.592|     2012649|    1569.3314|
| Liberia                  | Africa    |  1952|   38.480|      863308|     575.5730|
| Liberia                  | Africa    |  1957|   39.486|      975950|     620.9700|
| Liberia                  | Africa    |  1962|   40.502|     1112796|     634.1952|
| Liberia                  | Africa    |  1967|   41.536|     1279406|     713.6036|
| Liberia                  | Africa    |  1972|   42.614|     1482628|     803.0055|
| Liberia                  | Africa    |  1977|   43.764|     1703617|     640.3224|
| Liberia                  | Africa    |  1982|   44.852|     1956875|     572.1996|
| Liberia                  | Africa    |  1987|   46.027|     2269414|     506.1139|
| Liberia                  | Africa    |  1992|   40.802|     1912974|     636.6229|
| Liberia                  | Africa    |  1997|   42.221|     2200725|     609.1740|
| Liberia                  | Africa    |  2002|   43.753|     2814651|     531.4824|
| Liberia                  | Africa    |  2007|   45.678|     3193942|     414.5073|
| Libya                    | Africa    |  1952|   42.723|     1019729|    2387.5481|
| Libya                    | Africa    |  1957|   45.289|     1201578|    3448.2844|
| Libya                    | Africa    |  1962|   47.808|     1441863|    6757.0308|
| Libya                    | Africa    |  1967|   50.227|     1759224|   18772.7517|
| Libya                    | Africa    |  1972|   52.773|     2183877|   21011.4972|
| Libya                    | Africa    |  1977|   57.442|     2721783|   21951.2118|
| Libya                    | Africa    |  1982|   62.155|     3344074|   17364.2754|
| Libya                    | Africa    |  1987|   66.234|     3799845|   11770.5898|
| Libya                    | Africa    |  1992|   68.755|     4364501|    9640.1385|
| Libya                    | Africa    |  1997|   71.555|     4759670|    9467.4461|
| Libya                    | Africa    |  2002|   72.737|     5368585|    9534.6775|
| Libya                    | Africa    |  2007|   73.952|     6036914|   12057.4993|
| Madagascar               | Africa    |  1952|   36.681|     4762912|    1443.0117|
| Madagascar               | Africa    |  1957|   38.865|     5181679|    1589.2027|
| Madagascar               | Africa    |  1962|   40.848|     5703324|    1643.3871|
| Madagascar               | Africa    |  1967|   42.881|     6334556|    1634.0473|
| Madagascar               | Africa    |  1972|   44.851|     7082430|    1748.5630|
| Madagascar               | Africa    |  1977|   46.881|     8007166|    1544.2286|
| Madagascar               | Africa    |  1982|   48.969|     9171477|    1302.8787|
| Madagascar               | Africa    |  1987|   49.350|    10568642|    1155.4419|
| Madagascar               | Africa    |  1992|   52.214|    12210395|    1040.6762|
| Madagascar               | Africa    |  1997|   54.978|    14165114|     986.2959|
| Madagascar               | Africa    |  2002|   57.286|    16473477|     894.6371|
| Madagascar               | Africa    |  2007|   59.443|    19167654|    1044.7701|
| Malawi                   | Africa    |  1952|   36.256|     2917802|     369.1651|
| Malawi                   | Africa    |  1957|   37.207|     3221238|     416.3698|
| Malawi                   | Africa    |  1962|   38.410|     3628608|     427.9011|
| Malawi                   | Africa    |  1967|   39.487|     4147252|     495.5148|
| Malawi                   | Africa    |  1972|   41.766|     4730997|     584.6220|
| Malawi                   | Africa    |  1977|   43.767|     5637246|     663.2237|
| Malawi                   | Africa    |  1982|   45.642|     6502825|     632.8039|
| Malawi                   | Africa    |  1987|   47.457|     7824747|     635.5174|
| Malawi                   | Africa    |  1992|   49.420|    10014249|     563.2000|
| Malawi                   | Africa    |  1997|   47.495|    10419991|     692.2758|
| Malawi                   | Africa    |  2002|   45.009|    11824495|     665.4231|
| Malawi                   | Africa    |  2007|   48.303|    13327079|     759.3499|
| Malaysia                 | Asia      |  1952|   48.463|     6748378|    1831.1329|
| Malaysia                 | Asia      |  1957|   52.102|     7739235|    1810.0670|
| Malaysia                 | Asia      |  1962|   55.737|     8906385|    2036.8849|
| Malaysia                 | Asia      |  1967|   59.371|    10154878|    2277.7424|
| Malaysia                 | Asia      |  1972|   63.010|    11441462|    2849.0948|
| Malaysia                 | Asia      |  1977|   65.256|    12845381|    3827.9216|
| Malaysia                 | Asia      |  1982|   68.000|    14441916|    4920.3560|
| Malaysia                 | Asia      |  1987|   69.500|    16331785|    5249.8027|
| Malaysia                 | Asia      |  1992|   70.693|    18319502|    7277.9128|
| Malaysia                 | Asia      |  1997|   71.938|    20476091|   10132.9096|
| Malaysia                 | Asia      |  2002|   73.044|    22662365|   10206.9779|
| Malaysia                 | Asia      |  2007|   74.241|    24821286|   12451.6558|
| Mali                     | Africa    |  1952|   33.685|     3838168|     452.3370|
| Mali                     | Africa    |  1957|   35.307|     4241884|     490.3822|
| Mali                     | Africa    |  1962|   36.936|     4690372|     496.1743|
| Mali                     | Africa    |  1967|   38.487|     5212416|     545.0099|
| Mali                     | Africa    |  1972|   39.977|     5828158|     581.3689|
| Mali                     | Africa    |  1977|   41.714|     6491649|     686.3953|
| Mali                     | Africa    |  1982|   43.916|     6998256|     618.0141|
| Mali                     | Africa    |  1987|   46.364|     7634008|     684.1716|
| Mali                     | Africa    |  1992|   48.388|     8416215|     739.0144|
| Mali                     | Africa    |  1997|   49.903|     9384984|     790.2580|
| Mali                     | Africa    |  2002|   51.818|    10580176|     951.4098|
| Mali                     | Africa    |  2007|   54.467|    12031795|    1042.5816|
| Mauritania               | Africa    |  1952|   40.543|     1022556|     743.1159|
| Mauritania               | Africa    |  1957|   42.338|     1076852|     846.1203|
| Mauritania               | Africa    |  1962|   44.248|     1146757|    1055.8960|
| Mauritania               | Africa    |  1967|   46.289|     1230542|    1421.1452|
| Mauritania               | Africa    |  1972|   48.437|     1332786|    1586.8518|
| Mauritania               | Africa    |  1977|   50.852|     1456688|    1497.4922|
| Mauritania               | Africa    |  1982|   53.599|     1622136|    1481.1502|
| Mauritania               | Africa    |  1987|   56.145|     1841240|    1421.6036|
| Mauritania               | Africa    |  1992|   58.333|     2119465|    1361.3698|
| Mauritania               | Africa    |  1997|   60.430|     2444741|    1483.1361|
| Mauritania               | Africa    |  2002|   62.247|     2828858|    1579.0195|
| Mauritania               | Africa    |  2007|   64.164|     3270065|    1803.1515|
| Mauritius                | Africa    |  1952|   50.986|      516556|    1967.9557|
| Mauritius                | Africa    |  1957|   58.089|      609816|    2034.0380|
| Mauritius                | Africa    |  1962|   60.246|      701016|    2529.0675|
| Mauritius                | Africa    |  1967|   61.557|      789309|    2475.3876|
| Mauritius                | Africa    |  1972|   62.944|      851334|    2575.4842|
| Mauritius                | Africa    |  1977|   64.930|      913025|    3710.9830|
| Mauritius                | Africa    |  1982|   66.711|      992040|    3688.0377|
| Mauritius                | Africa    |  1987|   68.740|     1042663|    4783.5869|
| Mauritius                | Africa    |  1992|   69.745|     1096202|    6058.2538|
| Mauritius                | Africa    |  1997|   70.736|     1149818|    7425.7053|
| Mauritius                | Africa    |  2002|   71.954|     1200206|    9021.8159|
| Mauritius                | Africa    |  2007|   72.801|     1250882|   10956.9911|
| Mongolia                 | Asia      |  1952|   42.244|      800663|     786.5669|
| Mongolia                 | Asia      |  1957|   45.248|      882134|     912.6626|
| Mongolia                 | Asia      |  1962|   48.251|     1010280|    1056.3540|
| Mongolia                 | Asia      |  1967|   51.253|     1149500|    1226.0411|
| Mongolia                 | Asia      |  1972|   53.754|     1320500|    1421.7420|
| Mongolia                 | Asia      |  1977|   55.491|     1528000|    1647.5117|
| Mongolia                 | Asia      |  1982|   57.489|     1756032|    2000.6031|
| Mongolia                 | Asia      |  1987|   60.222|     2015133|    2338.0083|
| Mongolia                 | Asia      |  1992|   61.271|     2312802|    1785.4020|
| Mongolia                 | Asia      |  1997|   63.625|     2494803|    1902.2521|
| Mongolia                 | Asia      |  2002|   65.033|     2674234|    2140.7393|
| Mongolia                 | Asia      |  2007|   66.803|     2874127|    3095.7723|
| Montenegro               | Europe    |  1952|   59.164|      413834|    2647.5856|
| Montenegro               | Europe    |  1957|   61.448|      442829|    3682.2599|
| Montenegro               | Europe    |  1962|   63.728|      474528|    4649.5938|
| Montenegro               | Europe    |  1967|   67.178|      501035|    5907.8509|
| Montenegro               | Europe    |  1972|   70.636|      527678|    7778.4140|
| Montenegro               | Europe    |  1977|   73.066|      560073|    9595.9299|
| Montenegro               | Europe    |  1982|   74.101|      562548|   11222.5876|
| Montenegro               | Europe    |  1987|   74.865|      569473|   11732.5102|
| Montenegro               | Europe    |  1992|   75.435|      621621|    7003.3390|
| Montenegro               | Europe    |  1997|   75.445|      692651|    6465.6133|
| Montenegro               | Europe    |  2002|   73.981|      720230|    6557.1943|
| Montenegro               | Europe    |  2007|   74.543|      684736|    9253.8961|
| Morocco                  | Africa    |  1952|   42.873|     9939217|    1688.2036|
| Morocco                  | Africa    |  1957|   45.423|    11406350|    1642.0023|
| Morocco                  | Africa    |  1962|   47.924|    13056604|    1566.3535|
| Morocco                  | Africa    |  1967|   50.335|    14770296|    1711.0448|
| Morocco                  | Africa    |  1972|   52.862|    16660670|    1930.1950|
| Morocco                  | Africa    |  1977|   55.730|    18396941|    2370.6200|
| Morocco                  | Africa    |  1982|   59.650|    20198730|    2702.6204|
| Morocco                  | Africa    |  1987|   62.677|    22987397|    2755.0470|
| Morocco                  | Africa    |  1992|   65.393|    25798239|    2948.0473|
| Morocco                  | Africa    |  1997|   67.660|    28529501|    2982.1019|
| Morocco                  | Africa    |  2002|   69.615|    31167783|    3258.4956|
| Morocco                  | Africa    |  2007|   71.164|    33757175|    3820.1752|
| Mozambique               | Africa    |  1952|   31.286|     6446316|     468.5260|
| Mozambique               | Africa    |  1957|   33.779|     7038035|     495.5868|
| Mozambique               | Africa    |  1962|   36.161|     7788944|     556.6864|
| Mozambique               | Africa    |  1967|   38.113|     8680909|     566.6692|
| Mozambique               | Africa    |  1972|   40.328|     9809596|     724.9178|
| Mozambique               | Africa    |  1977|   42.495|    11127868|     502.3197|
| Mozambique               | Africa    |  1982|   42.795|    12587223|     462.2114|
| Mozambique               | Africa    |  1987|   42.861|    12891952|     389.8762|
| Mozambique               | Africa    |  1992|   44.284|    13160731|     410.8968|
| Mozambique               | Africa    |  1997|   46.344|    16603334|     472.3461|
| Mozambique               | Africa    |  2002|   44.026|    18473780|     633.6179|
| Mozambique               | Africa    |  2007|   42.082|    19951656|     823.6856|
| Myanmar                  | Asia      |  1952|   36.319|    20092996|     331.0000|
| Myanmar                  | Asia      |  1957|   41.905|    21731844|     350.0000|
| Myanmar                  | Asia      |  1962|   45.108|    23634436|     388.0000|
| Myanmar                  | Asia      |  1967|   49.379|    25870271|     349.0000|
| Myanmar                  | Asia      |  1972|   53.070|    28466390|     357.0000|
| Myanmar                  | Asia      |  1977|   56.059|    31528087|     371.0000|
| Myanmar                  | Asia      |  1982|   58.056|    34680442|     424.0000|
| Myanmar                  | Asia      |  1987|   58.339|    38028578|     385.0000|
| Myanmar                  | Asia      |  1992|   59.320|    40546538|     347.0000|
| Myanmar                  | Asia      |  1997|   60.328|    43247867|     415.0000|
| Myanmar                  | Asia      |  2002|   59.908|    45598081|     611.0000|
| Myanmar                  | Asia      |  2007|   62.069|    47761980|     944.0000|
| Namibia                  | Africa    |  1952|   41.725|      485831|    2423.7804|
| Namibia                  | Africa    |  1957|   45.226|      548080|    2621.4481|
| Namibia                  | Africa    |  1962|   48.386|      621392|    3173.2156|
| Namibia                  | Africa    |  1967|   51.159|      706640|    3793.6948|
| Namibia                  | Africa    |  1972|   53.867|      821782|    3746.0809|
| Namibia                  | Africa    |  1977|   56.437|      977026|    3876.4860|
| Namibia                  | Africa    |  1982|   58.968|     1099010|    4191.1005|
| Namibia                  | Africa    |  1987|   60.835|     1278184|    3693.7313|
| Namibia                  | Africa    |  1992|   61.999|     1554253|    3804.5380|
| Namibia                  | Africa    |  1997|   58.909|     1774766|    3899.5243|
| Namibia                  | Africa    |  2002|   51.479|     1972153|    4072.3248|
| Namibia                  | Africa    |  2007|   52.906|     2055080|    4811.0604|
| Nepal                    | Asia      |  1952|   36.157|     9182536|     545.8657|
| Nepal                    | Asia      |  1957|   37.686|     9682338|     597.9364|
| Nepal                    | Asia      |  1962|   39.393|    10332057|     652.3969|
| Nepal                    | Asia      |  1967|   41.472|    11261690|     676.4422|
| Nepal                    | Asia      |  1972|   43.971|    12412593|     674.7881|
| Nepal                    | Asia      |  1977|   46.748|    13933198|     694.1124|
| Nepal                    | Asia      |  1982|   49.594|    15796314|     718.3731|
| Nepal                    | Asia      |  1987|   52.537|    17917180|     775.6325|
| Nepal                    | Asia      |  1992|   55.727|    20326209|     897.7404|
| Nepal                    | Asia      |  1997|   59.426|    23001113|    1010.8921|
| Nepal                    | Asia      |  2002|   61.340|    25873917|    1057.2063|
| Nepal                    | Asia      |  2007|   63.785|    28901790|    1091.3598|
| Netherlands              | Europe    |  1952|   72.130|    10381988|    8941.5719|
| Netherlands              | Europe    |  1957|   72.990|    11026383|   11276.1934|
| Netherlands              | Europe    |  1962|   73.230|    11805689|   12790.8496|
| Netherlands              | Europe    |  1967|   73.820|    12596822|   15363.2514|
| Netherlands              | Europe    |  1972|   73.750|    13329874|   18794.7457|
| Netherlands              | Europe    |  1977|   75.240|    13852989|   21209.0592|
| Netherlands              | Europe    |  1982|   76.050|    14310401|   21399.4605|
| Netherlands              | Europe    |  1987|   76.830|    14665278|   23651.3236|
| Netherlands              | Europe    |  1992|   77.420|    15174244|   26790.9496|
| Netherlands              | Europe    |  1997|   78.030|    15604464|   30246.1306|
| Netherlands              | Europe    |  2002|   78.530|    16122830|   33724.7578|
| Netherlands              | Europe    |  2007|   79.762|    16570613|   36797.9333|
| Nicaragua                | Americas  |  1952|   42.314|     1165790|    3112.3639|
| Nicaragua                | Americas  |  1957|   45.432|     1358828|    3457.4159|
| Nicaragua                | Americas  |  1962|   48.632|     1590597|    3634.3644|
| Nicaragua                | Americas  |  1967|   51.884|     1865490|    4643.3935|
| Nicaragua                | Americas  |  1972|   55.151|     2182908|    4688.5933|
| Nicaragua                | Americas  |  1977|   57.470|     2554598|    5486.3711|
| Nicaragua                | Americas  |  1982|   59.298|     2979423|    3470.3382|
| Nicaragua                | Americas  |  1987|   62.008|     3344353|    2955.9844|
| Nicaragua                | Americas  |  1992|   65.843|     4017939|    2170.1517|
| Nicaragua                | Americas  |  1997|   68.426|     4609572|    2253.0230|
| Nicaragua                | Americas  |  2002|   70.836|     5146848|    2474.5488|
| Nicaragua                | Americas  |  2007|   72.899|     5675356|    2749.3210|
| Niger                    | Africa    |  1952|   37.444|     3379468|     761.8794|
| Niger                    | Africa    |  1957|   38.598|     3692184|     835.5234|
| Niger                    | Africa    |  1962|   39.487|     4076008|     997.7661|
| Niger                    | Africa    |  1967|   40.118|     4534062|    1054.3849|
| Niger                    | Africa    |  1972|   40.546|     5060262|     954.2092|
| Niger                    | Africa    |  1977|   41.291|     5682086|     808.8971|
| Niger                    | Africa    |  1982|   42.598|     6437188|     909.7221|
| Niger                    | Africa    |  1987|   44.555|     7332638|     668.3000|
| Niger                    | Africa    |  1992|   47.391|     8392818|     581.1827|
| Niger                    | Africa    |  1997|   51.313|     9666252|     580.3052|
| Niger                    | Africa    |  2002|   54.496|    11140655|     601.0745|
| Niger                    | Africa    |  2007|   56.867|    12894865|     619.6769|
| Nigeria                  | Africa    |  1952|   36.324|    33119096|    1077.2819|
| Nigeria                  | Africa    |  1957|   37.802|    37173340|    1100.5926|
| Nigeria                  | Africa    |  1962|   39.360|    41871351|    1150.9275|
| Nigeria                  | Africa    |  1967|   41.040|    47287752|    1014.5141|
| Nigeria                  | Africa    |  1972|   42.821|    53740085|    1698.3888|
| Nigeria                  | Africa    |  1977|   44.514|    62209173|    1981.9518|
| Nigeria                  | Africa    |  1982|   45.826|    73039376|    1576.9738|
| Nigeria                  | Africa    |  1987|   46.886|    81551520|    1385.0296|
| Nigeria                  | Africa    |  1992|   47.472|    93364244|    1619.8482|
| Nigeria                  | Africa    |  1997|   47.464|   106207839|    1624.9413|
| Nigeria                  | Africa    |  2002|   46.608|   119901274|    1615.2864|
| Nigeria                  | Africa    |  2007|   46.859|   135031164|    2013.9773|
| Oman                     | Asia      |  1952|   37.578|      507833|    1828.2303|
| Oman                     | Asia      |  1957|   40.080|      561977|    2242.7466|
| Oman                     | Asia      |  1962|   43.165|      628164|    2924.6381|
| Oman                     | Asia      |  1967|   46.988|      714775|    4720.9427|
| Oman                     | Asia      |  1972|   52.143|      829050|   10618.0385|
| Oman                     | Asia      |  1977|   57.367|     1004533|   11848.3439|
| Oman                     | Asia      |  1982|   62.728|     1301048|   12954.7910|
| Oman                     | Asia      |  1987|   67.734|     1593882|   18115.2231|
| Oman                     | Asia      |  1992|   71.197|     1915208|   18616.7069|
| Oman                     | Asia      |  1997|   72.499|     2283635|   19702.0558|
| Oman                     | Asia      |  2002|   74.193|     2713462|   19774.8369|
| Oman                     | Asia      |  2007|   75.640|     3204897|   22316.1929|
| Pakistan                 | Asia      |  1952|   43.436|    41346560|     684.5971|
| Pakistan                 | Asia      |  1957|   45.557|    46679944|     747.0835|
| Pakistan                 | Asia      |  1962|   47.670|    53100671|     803.3427|
| Pakistan                 | Asia      |  1967|   49.800|    60641899|     942.4083|
| Pakistan                 | Asia      |  1972|   51.929|    69325921|    1049.9390|
| Pakistan                 | Asia      |  1977|   54.043|    78152686|    1175.9212|
| Pakistan                 | Asia      |  1982|   56.158|    91462088|    1443.4298|
| Pakistan                 | Asia      |  1987|   58.245|   105186881|    1704.6866|
| Pakistan                 | Asia      |  1992|   60.838|   120065004|    1971.8295|
| Pakistan                 | Asia      |  1997|   61.818|   135564834|    2049.3505|
| Pakistan                 | Asia      |  2002|   63.610|   153403524|    2092.7124|
| Pakistan                 | Asia      |  2007|   65.483|   169270617|    2605.9476|
| Panama                   | Americas  |  1952|   55.191|      940080|    2480.3803|
| Panama                   | Americas  |  1957|   59.201|     1063506|    2961.8009|
| Panama                   | Americas  |  1962|   61.817|     1215725|    3536.5403|
| Panama                   | Americas  |  1967|   64.071|     1405486|    4421.0091|
| Panama                   | Americas  |  1972|   66.216|     1616384|    5364.2497|
| Panama                   | Americas  |  1977|   68.681|     1839782|    5351.9121|
| Panama                   | Americas  |  1982|   70.472|     2036305|    7009.6016|
| Panama                   | Americas  |  1987|   71.523|     2253639|    7034.7792|
| Panama                   | Americas  |  1992|   72.462|     2484997|    6618.7431|
| Panama                   | Americas  |  1997|   73.738|     2734531|    7113.6923|
| Panama                   | Americas  |  2002|   74.712|     2990875|    7356.0319|
| Panama                   | Americas  |  2007|   75.537|     3242173|    9809.1856|
| Paraguay                 | Americas  |  1952|   62.649|     1555876|    1952.3087|
| Paraguay                 | Americas  |  1957|   63.196|     1770902|    2046.1547|
| Paraguay                 | Americas  |  1962|   64.361|     2009813|    2148.0271|
| Paraguay                 | Americas  |  1967|   64.951|     2287985|    2299.3763|
| Paraguay                 | Americas  |  1972|   65.815|     2614104|    2523.3380|
| Paraguay                 | Americas  |  1977|   66.353|     2984494|    3248.3733|
| Paraguay                 | Americas  |  1982|   66.874|     3366439|    4258.5036|
| Paraguay                 | Americas  |  1987|   67.378|     3886512|    3998.8757|
| Paraguay                 | Americas  |  1992|   68.225|     4483945|    4196.4111|
| Paraguay                 | Americas  |  1997|   69.400|     5154123|    4247.4003|
| Paraguay                 | Americas  |  2002|   70.755|     5884491|    3783.6742|
| Paraguay                 | Americas  |  2007|   71.752|     6667147|    4172.8385|
| Peru                     | Americas  |  1952|   43.902|     8025700|    3758.5234|
| Peru                     | Americas  |  1957|   46.263|     9146100|    4245.2567|
| Peru                     | Americas  |  1962|   49.096|    10516500|    4957.0380|
| Peru                     | Americas  |  1967|   51.445|    12132200|    5788.0933|
| Peru                     | Americas  |  1972|   55.448|    13954700|    5937.8273|
| Peru                     | Americas  |  1977|   58.447|    15990099|    6281.2909|
| Peru                     | Americas  |  1982|   61.406|    18125129|    6434.5018|
| Peru                     | Americas  |  1987|   64.134|    20195924|    6360.9434|
| Peru                     | Americas  |  1992|   66.458|    22430449|    4446.3809|
| Peru                     | Americas  |  1997|   68.386|    24748122|    5838.3477|
| Peru                     | Americas  |  2002|   69.906|    26769436|    5909.0201|
| Peru                     | Americas  |  2007|   71.421|    28674757|    7408.9056|
| Philippines              | Asia      |  1952|   47.752|    22438691|    1272.8810|
| Philippines              | Asia      |  1957|   51.334|    26072194|    1547.9448|
| Philippines              | Asia      |  1962|   54.757|    30325264|    1649.5522|
| Philippines              | Asia      |  1967|   56.393|    35356600|    1814.1274|
| Philippines              | Asia      |  1972|   58.065|    40850141|    1989.3741|
| Philippines              | Asia      |  1977|   60.060|    46850962|    2373.2043|
| Philippines              | Asia      |  1982|   62.082|    53456774|    2603.2738|
| Philippines              | Asia      |  1987|   64.151|    60017788|    2189.6350|
| Philippines              | Asia      |  1992|   66.458|    67185766|    2279.3240|
| Philippines              | Asia      |  1997|   68.564|    75012988|    2536.5349|
| Philippines              | Asia      |  2002|   70.303|    82995088|    2650.9211|
| Philippines              | Asia      |  2007|   71.688|    91077287|    3190.4810|
| Poland                   | Europe    |  1952|   61.310|    25730551|    4029.3297|
| Poland                   | Europe    |  1957|   65.770|    28235346|    4734.2530|
| Poland                   | Europe    |  1962|   67.640|    30329617|    5338.7521|
| Poland                   | Europe    |  1967|   69.610|    31785378|    6557.1528|
| Poland                   | Europe    |  1972|   70.850|    33039545|    8006.5070|
| Poland                   | Europe    |  1977|   70.670|    34621254|    9508.1415|
| Poland                   | Europe    |  1982|   71.320|    36227381|    8451.5310|
| Poland                   | Europe    |  1987|   70.980|    37740710|    9082.3512|
| Poland                   | Europe    |  1992|   70.990|    38370697|    7738.8812|
| Poland                   | Europe    |  1997|   72.750|    38654957|   10159.5837|
| Poland                   | Europe    |  2002|   74.670|    38625976|   12002.2391|
| Poland                   | Europe    |  2007|   75.563|    38518241|   15389.9247|
| Portugal                 | Europe    |  1952|   59.820|     8526050|    3068.3199|
| Portugal                 | Europe    |  1957|   61.510|     8817650|    3774.5717|
| Portugal                 | Europe    |  1962|   64.390|     9019800|    4727.9549|
| Portugal                 | Europe    |  1967|   66.600|     9103000|    6361.5180|
| Portugal                 | Europe    |  1972|   69.260|     8970450|    9022.2474|
| Portugal                 | Europe    |  1977|   70.410|     9662600|   10172.4857|
| Portugal                 | Europe    |  1982|   72.770|     9859650|   11753.8429|
| Portugal                 | Europe    |  1987|   74.060|     9915289|   13039.3088|
| Portugal                 | Europe    |  1992|   74.860|     9927680|   16207.2666|
| Portugal                 | Europe    |  1997|   75.970|    10156415|   17641.0316|
| Portugal                 | Europe    |  2002|   77.290|    10433867|   19970.9079|
| Portugal                 | Europe    |  2007|   78.098|    10642836|   20509.6478|
| Puerto Rico              | Americas  |  1952|   64.280|     2227000|    3081.9598|
| Puerto Rico              | Americas  |  1957|   68.540|     2260000|    3907.1562|
| Puerto Rico              | Americas  |  1962|   69.620|     2448046|    5108.3446|
| Puerto Rico              | Americas  |  1967|   71.100|     2648961|    6929.2777|
| Puerto Rico              | Americas  |  1972|   72.160|     2847132|    9123.0417|
| Puerto Rico              | Americas  |  1977|   73.440|     3080828|    9770.5249|
| Puerto Rico              | Americas  |  1982|   73.750|     3279001|   10330.9891|
| Puerto Rico              | Americas  |  1987|   74.630|     3444468|   12281.3419|
| Puerto Rico              | Americas  |  1992|   73.911|     3585176|   14641.5871|
| Puerto Rico              | Americas  |  1997|   74.917|     3759430|   16999.4333|
| Puerto Rico              | Americas  |  2002|   77.778|     3859606|   18855.6062|
| Puerto Rico              | Americas  |  2007|   78.746|     3942491|   19328.7090|
| Reunion                  | Africa    |  1952|   52.724|      257700|    2718.8853|
| Reunion                  | Africa    |  1957|   55.090|      308700|    2769.4518|
| Reunion                  | Africa    |  1962|   57.666|      358900|    3173.7233|
| Reunion                  | Africa    |  1967|   60.542|      414024|    4021.1757|
| Reunion                  | Africa    |  1972|   64.274|      461633|    5047.6586|
| Reunion                  | Africa    |  1977|   67.064|      492095|    4319.8041|
| Reunion                  | Africa    |  1982|   69.885|      517810|    5267.2194|
| Reunion                  | Africa    |  1987|   71.913|      562035|    5303.3775|
| Reunion                  | Africa    |  1992|   73.615|      622191|    6101.2558|
| Reunion                  | Africa    |  1997|   74.772|      684810|    6071.9414|
| Reunion                  | Africa    |  2002|   75.744|      743981|    6316.1652|
| Reunion                  | Africa    |  2007|   76.442|      798094|    7670.1226|
| Romania                  | Europe    |  1952|   61.050|    16630000|    3144.6132|
| Romania                  | Europe    |  1957|   64.100|    17829327|    3943.3702|
| Romania                  | Europe    |  1962|   66.800|    18680721|    4734.9976|
| Romania                  | Europe    |  1967|   66.800|    19284814|    6470.8665|
| Romania                  | Europe    |  1972|   69.210|    20662648|    8011.4144|
| Romania                  | Europe    |  1977|   69.460|    21658597|    9356.3972|
| Romania                  | Europe    |  1982|   69.660|    22356726|    9605.3141|
| Romania                  | Europe    |  1987|   69.530|    22686371|    9696.2733|
| Romania                  | Europe    |  1992|   69.360|    22797027|    6598.4099|
| Romania                  | Europe    |  1997|   69.720|    22562458|    7346.5476|
| Romania                  | Europe    |  2002|   71.322|    22404337|    7885.3601|
| Romania                  | Europe    |  2007|   72.476|    22276056|   10808.4756|
| Rwanda                   | Africa    |  1952|   40.000|     2534927|     493.3239|
| Rwanda                   | Africa    |  1957|   41.500|     2822082|     540.2894|
| Rwanda                   | Africa    |  1962|   43.000|     3051242|     597.4731|
| Rwanda                   | Africa    |  1967|   44.100|     3451079|     510.9637|
| Rwanda                   | Africa    |  1972|   44.600|     3992121|     590.5807|
| Rwanda                   | Africa    |  1977|   45.000|     4657072|     670.0806|
| Rwanda                   | Africa    |  1982|   46.218|     5507565|     881.5706|
| Rwanda                   | Africa    |  1987|   44.020|     6349365|     847.9912|
| Rwanda                   | Africa    |  1992|   23.599|     7290203|     737.0686|
| Rwanda                   | Africa    |  1997|   36.087|     7212583|     589.9445|
| Rwanda                   | Africa    |  2002|   43.413|     7852401|     785.6538|
| Rwanda                   | Africa    |  2007|   46.242|     8860588|     863.0885|
| Sao Tome and Principe    | Africa    |  1952|   46.471|       60011|     879.5836|
| Sao Tome and Principe    | Africa    |  1957|   48.945|       61325|     860.7369|
| Sao Tome and Principe    | Africa    |  1962|   51.893|       65345|    1071.5511|
| Sao Tome and Principe    | Africa    |  1967|   54.425|       70787|    1384.8406|
| Sao Tome and Principe    | Africa    |  1972|   56.480|       76595|    1532.9853|
| Sao Tome and Principe    | Africa    |  1977|   58.550|       86796|    1737.5617|
| Sao Tome and Principe    | Africa    |  1982|   60.351|       98593|    1890.2181|
| Sao Tome and Principe    | Africa    |  1987|   61.728|      110812|    1516.5255|
| Sao Tome and Principe    | Africa    |  1992|   62.742|      125911|    1428.7778|
| Sao Tome and Principe    | Africa    |  1997|   63.306|      145608|    1339.0760|
| Sao Tome and Principe    | Africa    |  2002|   64.337|      170372|    1353.0924|
| Sao Tome and Principe    | Africa    |  2007|   65.528|      199579|    1598.4351|
| Saudi Arabia             | Asia      |  1952|   39.875|     4005677|    6459.5548|
| Saudi Arabia             | Asia      |  1957|   42.868|     4419650|    8157.5912|
| Saudi Arabia             | Asia      |  1962|   45.914|     4943029|   11626.4197|
| Saudi Arabia             | Asia      |  1967|   49.901|     5618198|   16903.0489|
| Saudi Arabia             | Asia      |  1972|   53.886|     6472756|   24837.4287|
| Saudi Arabia             | Asia      |  1977|   58.690|     8128505|   34167.7626|
| Saudi Arabia             | Asia      |  1982|   63.012|    11254672|   33693.1753|
| Saudi Arabia             | Asia      |  1987|   66.295|    14619745|   21198.2614|
| Saudi Arabia             | Asia      |  1992|   68.768|    16945857|   24841.6178|
| Saudi Arabia             | Asia      |  1997|   70.533|    21229759|   20586.6902|
| Saudi Arabia             | Asia      |  2002|   71.626|    24501530|   19014.5412|
| Saudi Arabia             | Asia      |  2007|   72.777|    27601038|   21654.8319|
| Senegal                  | Africa    |  1952|   37.278|     2755589|    1450.3570|
| Senegal                  | Africa    |  1957|   39.329|     3054547|    1567.6530|
| Senegal                  | Africa    |  1962|   41.454|     3430243|    1654.9887|
| Senegal                  | Africa    |  1967|   43.563|     3965841|    1612.4046|
| Senegal                  | Africa    |  1972|   45.815|     4588696|    1597.7121|
| Senegal                  | Africa    |  1977|   48.879|     5260855|    1561.7691|
| Senegal                  | Africa    |  1982|   52.379|     6147783|    1518.4800|
| Senegal                  | Africa    |  1987|   55.769|     7171347|    1441.7207|
| Senegal                  | Africa    |  1992|   58.196|     8307920|    1367.8994|
| Senegal                  | Africa    |  1997|   60.187|     9535314|    1392.3683|
| Senegal                  | Africa    |  2002|   61.600|    10870037|    1519.6353|
| Senegal                  | Africa    |  2007|   63.062|    12267493|    1712.4721|
| Serbia                   | Europe    |  1952|   57.996|     6860147|    3581.4594|
| Serbia                   | Europe    |  1957|   61.685|     7271135|    4981.0909|
| Serbia                   | Europe    |  1962|   64.531|     7616060|    6289.6292|
| Serbia                   | Europe    |  1967|   66.914|     7971222|    7991.7071|
| Serbia                   | Europe    |  1972|   68.700|     8313288|   10522.0675|
| Serbia                   | Europe    |  1977|   70.300|     8686367|   12980.6696|
| Serbia                   | Europe    |  1982|   70.162|     9032824|   15181.0927|
| Serbia                   | Europe    |  1987|   71.218|     9230783|   15870.8785|
| Serbia                   | Europe    |  1992|   71.659|     9826397|    9325.0682|
| Serbia                   | Europe    |  1997|   72.232|    10336594|    7914.3203|
| Serbia                   | Europe    |  2002|   73.213|    10111559|    7236.0753|
| Serbia                   | Europe    |  2007|   74.002|    10150265|    9786.5347|
| Sierra Leone             | Africa    |  1952|   30.331|     2143249|     879.7877|
| Sierra Leone             | Africa    |  1957|   31.570|     2295678|    1004.4844|
| Sierra Leone             | Africa    |  1962|   32.767|     2467895|    1116.6399|
| Sierra Leone             | Africa    |  1967|   34.113|     2662190|    1206.0435|
| Sierra Leone             | Africa    |  1972|   35.400|     2879013|    1353.7598|
| Sierra Leone             | Africa    |  1977|   36.788|     3140897|    1348.2852|
| Sierra Leone             | Africa    |  1982|   38.445|     3464522|    1465.0108|
| Sierra Leone             | Africa    |  1987|   40.006|     3868905|    1294.4478|
| Sierra Leone             | Africa    |  1992|   38.333|     4260884|    1068.6963|
| Sierra Leone             | Africa    |  1997|   39.897|     4578212|     574.6482|
| Sierra Leone             | Africa    |  2002|   41.012|     5359092|     699.4897|
| Sierra Leone             | Africa    |  2007|   42.568|     6144562|     862.5408|
| Singapore                | Asia      |  1952|   60.396|     1127000|    2315.1382|
| Singapore                | Asia      |  1957|   63.179|     1445929|    2843.1044|
| Singapore                | Asia      |  1962|   65.798|     1750200|    3674.7356|
| Singapore                | Asia      |  1967|   67.946|     1977600|    4977.4185|
| Singapore                | Asia      |  1972|   69.521|     2152400|    8597.7562|
| Singapore                | Asia      |  1977|   70.795|     2325300|   11210.0895|
| Singapore                | Asia      |  1982|   71.760|     2651869|   15169.1611|
| Singapore                | Asia      |  1987|   73.560|     2794552|   18861.5308|
| Singapore                | Asia      |  1992|   75.788|     3235865|   24769.8912|
| Singapore                | Asia      |  1997|   77.158|     3802309|   33519.4766|
| Singapore                | Asia      |  2002|   78.770|     4197776|   36023.1054|
| Singapore                | Asia      |  2007|   79.972|     4553009|   47143.1796|
| Slovak Republic          | Europe    |  1952|   64.360|     3558137|    5074.6591|
| Slovak Republic          | Europe    |  1957|   67.450|     3844277|    6093.2630|
| Slovak Republic          | Europe    |  1962|   70.330|     4237384|    7481.1076|
| Slovak Republic          | Europe    |  1967|   70.980|     4442238|    8412.9024|
| Slovak Republic          | Europe    |  1972|   70.350|     4593433|    9674.1676|
| Slovak Republic          | Europe    |  1977|   70.450|     4827803|   10922.6640|
| Slovak Republic          | Europe    |  1982|   70.800|     5048043|   11348.5459|
| Slovak Republic          | Europe    |  1987|   71.080|     5199318|   12037.2676|
| Slovak Republic          | Europe    |  1992|   71.380|     5302888|    9498.4677|
| Slovak Republic          | Europe    |  1997|   72.710|     5383010|   12126.2306|
| Slovak Republic          | Europe    |  2002|   73.800|     5410052|   13638.7784|
| Slovak Republic          | Europe    |  2007|   74.663|     5447502|   18678.3144|
| Slovenia                 | Europe    |  1952|   65.570|     1489518|    4215.0417|
| Slovenia                 | Europe    |  1957|   67.850|     1533070|    5862.2766|
| Slovenia                 | Europe    |  1962|   69.150|     1582962|    7402.3034|
| Slovenia                 | Europe    |  1967|   69.180|     1646912|    9405.4894|
| Slovenia                 | Europe    |  1972|   69.820|     1694510|   12383.4862|
| Slovenia                 | Europe    |  1977|   70.970|     1746919|   15277.0302|
| Slovenia                 | Europe    |  1982|   71.063|     1861252|   17866.7218|
| Slovenia                 | Europe    |  1987|   72.250|     1945870|   18678.5349|
| Slovenia                 | Europe    |  1992|   73.640|     1999210|   14214.7168|
| Slovenia                 | Europe    |  1997|   75.130|     2011612|   17161.1073|
| Slovenia                 | Europe    |  2002|   76.660|     2011497|   20660.0194|
| Slovenia                 | Europe    |  2007|   77.926|     2009245|   25768.2576|
| South Africa             | Africa    |  1952|   45.009|    14264935|    4725.2955|
| South Africa             | Africa    |  1957|   47.985|    16151549|    5487.1042|
| South Africa             | Africa    |  1962|   49.951|    18356657|    5768.7297|
| South Africa             | Africa    |  1967|   51.927|    20997321|    7114.4780|
| South Africa             | Africa    |  1972|   53.696|    23935810|    7765.9626|
| South Africa             | Africa    |  1977|   55.527|    27129932|    8028.6514|
| South Africa             | Africa    |  1982|   58.161|    31140029|    8568.2662|
| South Africa             | Africa    |  1987|   60.834|    35933379|    7825.8234|
| South Africa             | Africa    |  1992|   61.888|    39964159|    7225.0693|
| South Africa             | Africa    |  1997|   60.236|    42835005|    7479.1882|
| South Africa             | Africa    |  2002|   53.365|    44433622|    7710.9464|
| South Africa             | Africa    |  2007|   49.339|    43997828|    9269.6578|
| Spain                    | Europe    |  1952|   64.940|    28549870|    3834.0347|
| Spain                    | Europe    |  1957|   66.660|    29841614|    4564.8024|
| Spain                    | Europe    |  1962|   69.690|    31158061|    5693.8439|
| Spain                    | Europe    |  1967|   71.440|    32850275|    7993.5123|
| Spain                    | Europe    |  1972|   73.060|    34513161|   10638.7513|
| Spain                    | Europe    |  1977|   74.390|    36439000|   13236.9212|
| Spain                    | Europe    |  1982|   76.300|    37983310|   13926.1700|
| Spain                    | Europe    |  1987|   76.900|    38880702|   15764.9831|
| Spain                    | Europe    |  1992|   77.570|    39549438|   18603.0645|
| Spain                    | Europe    |  1997|   78.770|    39855442|   20445.2990|
| Spain                    | Europe    |  2002|   79.780|    40152517|   24835.4717|
| Spain                    | Europe    |  2007|   80.941|    40448191|   28821.0637|
| Sri Lanka                | Asia      |  1952|   57.593|     7982342|    1083.5320|
| Sri Lanka                | Asia      |  1957|   61.456|     9128546|    1072.5466|
| Sri Lanka                | Asia      |  1962|   62.192|    10421936|    1074.4720|
| Sri Lanka                | Asia      |  1967|   64.266|    11737396|    1135.5143|
| Sri Lanka                | Asia      |  1972|   65.042|    13016733|    1213.3955|
| Sri Lanka                | Asia      |  1977|   65.949|    14116836|    1348.7757|
| Sri Lanka                | Asia      |  1982|   68.757|    15410151|    1648.0798|
| Sri Lanka                | Asia      |  1987|   69.011|    16495304|    1876.7668|
| Sri Lanka                | Asia      |  1992|   70.379|    17587060|    2153.7392|
| Sri Lanka                | Asia      |  1997|   70.457|    18698655|    2664.4773|
| Sri Lanka                | Asia      |  2002|   70.815|    19576783|    3015.3788|
| Sri Lanka                | Asia      |  2007|   72.396|    20378239|    3970.0954|
| Sudan                    | Africa    |  1952|   38.635|     8504667|    1615.9911|
| Sudan                    | Africa    |  1957|   39.624|     9753392|    1770.3371|
| Sudan                    | Africa    |  1962|   40.870|    11183227|    1959.5938|
| Sudan                    | Africa    |  1967|   42.858|    12716129|    1687.9976|
| Sudan                    | Africa    |  1972|   45.083|    14597019|    1659.6528|
| Sudan                    | Africa    |  1977|   47.800|    17104986|    2202.9884|
| Sudan                    | Africa    |  1982|   50.338|    20367053|    1895.5441|
| Sudan                    | Africa    |  1987|   51.744|    24725960|    1507.8192|
| Sudan                    | Africa    |  1992|   53.556|    28227588|    1492.1970|
| Sudan                    | Africa    |  1997|   55.373|    32160729|    1632.2108|
| Sudan                    | Africa    |  2002|   56.369|    37090298|    1993.3983|
| Sudan                    | Africa    |  2007|   58.556|    42292929|    2602.3950|
| Swaziland                | Africa    |  1952|   41.407|      290243|    1148.3766|
| Swaziland                | Africa    |  1957|   43.424|      326741|    1244.7084|
| Swaziland                | Africa    |  1962|   44.992|      370006|    1856.1821|
| Swaziland                | Africa    |  1967|   46.633|      420690|    2613.1017|
| Swaziland                | Africa    |  1972|   49.552|      480105|    3364.8366|
| Swaziland                | Africa    |  1977|   52.537|      551425|    3781.4106|
| Swaziland                | Africa    |  1982|   55.561|      649901|    3895.3840|
| Swaziland                | Africa    |  1987|   57.678|      779348|    3984.8398|
| Swaziland                | Africa    |  1992|   58.474|      962344|    3553.0224|
| Swaziland                | Africa    |  1997|   54.289|     1054486|    3876.7685|
| Swaziland                | Africa    |  2002|   43.869|     1130269|    4128.1169|
| Swaziland                | Africa    |  2007|   39.613|     1133066|    4513.4806|
| Sweden                   | Europe    |  1952|   71.860|     7124673|    8527.8447|
| Sweden                   | Europe    |  1957|   72.490|     7363802|    9911.8782|
| Sweden                   | Europe    |  1962|   73.370|     7561588|   12329.4419|
| Sweden                   | Europe    |  1967|   74.160|     7867931|   15258.2970|
| Sweden                   | Europe    |  1972|   74.720|     8122293|   17832.0246|
| Sweden                   | Europe    |  1977|   75.440|     8251648|   18855.7252|
| Sweden                   | Europe    |  1982|   76.420|     8325260|   20667.3812|
| Sweden                   | Europe    |  1987|   77.190|     8421403|   23586.9293|
| Sweden                   | Europe    |  1992|   78.160|     8718867|   23880.0168|
| Sweden                   | Europe    |  1997|   79.390|     8897619|   25266.5950|
| Sweden                   | Europe    |  2002|   80.040|     8954175|   29341.6309|
| Sweden                   | Europe    |  2007|   80.884|     9031088|   33859.7484|
| Switzerland              | Europe    |  1952|   69.620|     4815000|   14734.2327|
| Switzerland              | Europe    |  1957|   70.560|     5126000|   17909.4897|
| Switzerland              | Europe    |  1962|   71.320|     5666000|   20431.0927|
| Switzerland              | Europe    |  1967|   72.770|     6063000|   22966.1443|
| Switzerland              | Europe    |  1972|   73.780|     6401400|   27195.1130|
| Switzerland              | Europe    |  1977|   75.390|     6316424|   26982.2905|
| Switzerland              | Europe    |  1982|   76.210|     6468126|   28397.7151|
| Switzerland              | Europe    |  1987|   77.410|     6649942|   30281.7046|
| Switzerland              | Europe    |  1992|   78.030|     6995447|   31871.5303|
| Switzerland              | Europe    |  1997|   79.370|     7193761|   32135.3230|
| Switzerland              | Europe    |  2002|   80.620|     7361757|   34480.9577|
| Switzerland              | Europe    |  2007|   81.701|     7554661|   37506.4191|
| Syria                    | Asia      |  1952|   45.883|     3661549|    1643.4854|
| Syria                    | Asia      |  1957|   48.284|     4149908|    2117.2349|
| Syria                    | Asia      |  1962|   50.305|     4834621|    2193.0371|
| Syria                    | Asia      |  1967|   53.655|     5680812|    1881.9236|
| Syria                    | Asia      |  1972|   57.296|     6701172|    2571.4230|
| Syria                    | Asia      |  1977|   61.195|     7932503|    3195.4846|
| Syria                    | Asia      |  1982|   64.590|     9410494|    3761.8377|
| Syria                    | Asia      |  1987|   66.974|    11242847|    3116.7743|
| Syria                    | Asia      |  1992|   69.249|    13219062|    3340.5428|
| Syria                    | Asia      |  1997|   71.527|    15081016|    4014.2390|
| Syria                    | Asia      |  2002|   73.053|    17155814|    4090.9253|
| Syria                    | Asia      |  2007|   74.143|    19314747|    4184.5481|
| Taiwan                   | Asia      |  1952|   58.500|     8550362|    1206.9479|
| Taiwan                   | Asia      |  1957|   62.400|    10164215|    1507.8613|
| Taiwan                   | Asia      |  1962|   65.200|    11918938|    1822.8790|
| Taiwan                   | Asia      |  1967|   67.500|    13648692|    2643.8587|
| Taiwan                   | Asia      |  1972|   69.390|    15226039|    4062.5239|
| Taiwan                   | Asia      |  1977|   70.590|    16785196|    5596.5198|
| Taiwan                   | Asia      |  1982|   72.160|    18501390|    7426.3548|
| Taiwan                   | Asia      |  1987|   73.400|    19757799|   11054.5618|
| Taiwan                   | Asia      |  1992|   74.260|    20686918|   15215.6579|
| Taiwan                   | Asia      |  1997|   75.250|    21628605|   20206.8210|
| Taiwan                   | Asia      |  2002|   76.990|    22454239|   23235.4233|
| Taiwan                   | Asia      |  2007|   78.400|    23174294|   28718.2768|
| Tanzania                 | Africa    |  1952|   41.215|     8322925|     716.6501|
| Tanzania                 | Africa    |  1957|   42.974|     9452826|     698.5356|
| Tanzania                 | Africa    |  1962|   44.246|    10863958|     722.0038|
| Tanzania                 | Africa    |  1967|   45.757|    12607312|     848.2187|
| Tanzania                 | Africa    |  1972|   47.620|    14706593|     915.9851|
| Tanzania                 | Africa    |  1977|   49.919|    17129565|     962.4923|
| Tanzania                 | Africa    |  1982|   50.608|    19844382|     874.2426|
| Tanzania                 | Africa    |  1987|   51.535|    23040630|     831.8221|
| Tanzania                 | Africa    |  1992|   50.440|    26605473|     825.6825|
| Tanzania                 | Africa    |  1997|   48.466|    30686889|     789.1862|
| Tanzania                 | Africa    |  2002|   49.651|    34593779|     899.0742|
| Tanzania                 | Africa    |  2007|   52.517|    38139640|    1107.4822|
| Togo                     | Africa    |  1952|   38.596|     1219113|     859.8087|
| Togo                     | Africa    |  1957|   41.208|     1357445|     925.9083|
| Togo                     | Africa    |  1962|   43.922|     1528098|    1067.5348|
| Togo                     | Africa    |  1967|   46.769|     1735550|    1477.5968|
| Togo                     | Africa    |  1972|   49.759|     2056351|    1649.6602|
| Togo                     | Africa    |  1977|   52.887|     2308582|    1532.7770|
| Togo                     | Africa    |  1982|   55.471|     2644765|    1344.5780|
| Togo                     | Africa    |  1987|   56.941|     3154264|    1202.2014|
| Togo                     | Africa    |  1992|   58.061|     3747553|    1034.2989|
| Togo                     | Africa    |  1997|   58.390|     4320890|     982.2869|
| Togo                     | Africa    |  2002|   57.561|     4977378|     886.2206|
| Togo                     | Africa    |  2007|   58.420|     5701579|     882.9699|
| Trinidad and Tobago      | Americas  |  1952|   59.100|      662850|    3023.2719|
| Trinidad and Tobago      | Americas  |  1957|   61.800|      764900|    4100.3934|
| Trinidad and Tobago      | Americas  |  1962|   64.900|      887498|    4997.5240|
| Trinidad and Tobago      | Americas  |  1967|   65.400|      960155|    5621.3685|
| Trinidad and Tobago      | Americas  |  1972|   65.900|      975199|    6619.5514|
| Trinidad and Tobago      | Americas  |  1977|   68.300|     1039009|    7899.5542|
| Trinidad and Tobago      | Americas  |  1982|   68.832|     1116479|    9119.5286|
| Trinidad and Tobago      | Americas  |  1987|   69.582|     1191336|    7388.5978|
| Trinidad and Tobago      | Americas  |  1992|   69.862|     1183669|    7370.9909|
| Trinidad and Tobago      | Americas  |  1997|   69.465|     1138101|    8792.5731|
| Trinidad and Tobago      | Americas  |  2002|   68.976|     1101832|   11460.6002|
| Trinidad and Tobago      | Americas  |  2007|   69.819|     1056608|   18008.5092|
| Tunisia                  | Africa    |  1952|   44.600|     3647735|    1468.4756|
| Tunisia                  | Africa    |  1957|   47.100|     3950849|    1395.2325|
| Tunisia                  | Africa    |  1962|   49.579|     4286552|    1660.3032|
| Tunisia                  | Africa    |  1967|   52.053|     4786986|    1932.3602|
| Tunisia                  | Africa    |  1972|   55.602|     5303507|    2753.2860|
| Tunisia                  | Africa    |  1977|   59.837|     6005061|    3120.8768|
| Tunisia                  | Africa    |  1982|   64.048|     6734098|    3560.2332|
| Tunisia                  | Africa    |  1987|   66.894|     7724976|    3810.4193|
| Tunisia                  | Africa    |  1992|   70.001|     8523077|    4332.7202|
| Tunisia                  | Africa    |  1997|   71.973|     9231669|    4876.7986|
| Tunisia                  | Africa    |  2002|   73.042|     9770575|    5722.8957|
| Tunisia                  | Africa    |  2007|   73.923|    10276158|    7092.9230|
| Uganda                   | Africa    |  1952|   39.978|     5824797|     734.7535|
| Uganda                   | Africa    |  1957|   42.571|     6675501|     774.3711|
| Uganda                   | Africa    |  1962|   45.344|     7688797|     767.2717|
| Uganda                   | Africa    |  1967|   48.051|     8900294|     908.9185|
| Uganda                   | Africa    |  1972|   51.016|    10190285|     950.7359|
| Uganda                   | Africa    |  1977|   50.350|    11457758|     843.7331|
| Uganda                   | Africa    |  1982|   49.849|    12939400|     682.2662|
| Uganda                   | Africa    |  1987|   51.509|    15283050|     617.7244|
| Uganda                   | Africa    |  1992|   48.825|    18252190|     644.1708|
| Uganda                   | Africa    |  1997|   44.578|    21210254|     816.5591|
| Uganda                   | Africa    |  2002|   47.813|    24739869|     927.7210|
| Uganda                   | Africa    |  2007|   51.542|    29170398|    1056.3801|
| United Kingdom           | Europe    |  1952|   69.180|    50430000|    9979.5085|
| United Kingdom           | Europe    |  1957|   70.420|    51430000|   11283.1779|
| United Kingdom           | Europe    |  1962|   70.760|    53292000|   12477.1771|
| United Kingdom           | Europe    |  1967|   71.360|    54959000|   14142.8509|
| United Kingdom           | Europe    |  1972|   72.010|    56079000|   15895.1164|
| United Kingdom           | Europe    |  1977|   72.760|    56179000|   17428.7485|
| United Kingdom           | Europe    |  1982|   74.040|    56339704|   18232.4245|
| United Kingdom           | Europe    |  1987|   75.007|    56981620|   21664.7877|
| United Kingdom           | Europe    |  1992|   76.420|    57866349|   22705.0925|
| United Kingdom           | Europe    |  1997|   77.218|    58808266|   26074.5314|
| United Kingdom           | Europe    |  2002|   78.471|    59912431|   29478.9992|
| United Kingdom           | Europe    |  2007|   79.425|    60776238|   33203.2613|
| Venezuela                | Americas  |  1952|   55.088|     5439568|    7689.7998|
| Venezuela                | Americas  |  1957|   57.907|     6702668|    9802.4665|
| Venezuela                | Americas  |  1962|   60.770|     8143375|    8422.9742|
| Venezuela                | Americas  |  1967|   63.479|     9709552|    9541.4742|
| Venezuela                | Americas  |  1972|   65.712|    11515649|   10505.2597|
| Venezuela                | Americas  |  1977|   67.456|    13503563|   13143.9510|
| Venezuela                | Americas  |  1982|   68.557|    15620766|   11152.4101|
| Venezuela                | Americas  |  1987|   70.190|    17910182|    9883.5846|
| Venezuela                | Americas  |  1992|   71.150|    20265563|   10733.9263|
| Venezuela                | Americas  |  1997|   72.146|    22374398|   10165.4952|
| Venezuela                | Americas  |  2002|   72.766|    24287670|    8605.0478|
| Venezuela                | Americas  |  2007|   73.747|    26084662|   11415.8057|
| Vietnam                  | Asia      |  1952|   40.412|    26246839|     605.0665|
| Vietnam                  | Asia      |  1957|   42.887|    28998543|     676.2854|
| Vietnam                  | Asia      |  1962|   45.363|    33796140|     772.0492|
| Vietnam                  | Asia      |  1967|   47.838|    39463910|     637.1233|
| Vietnam                  | Asia      |  1972|   50.254|    44655014|     699.5016|
| Vietnam                  | Asia      |  1977|   55.764|    50533506|     713.5371|
| Vietnam                  | Asia      |  1982|   58.816|    56142181|     707.2358|
| Vietnam                  | Asia      |  1987|   62.820|    62826491|     820.7994|
| Vietnam                  | Asia      |  1992|   67.662|    69940728|     989.0231|
| Vietnam                  | Asia      |  1997|   70.672|    76048996|    1385.8968|
| Vietnam                  | Asia      |  2002|   73.017|    80908147|    1764.4567|
| Vietnam                  | Asia      |  2007|   74.249|    85262356|    2441.5764|
| West Bank and Gaza       | Asia      |  1952|   43.160|     1030585|    1515.5923|
| West Bank and Gaza       | Asia      |  1957|   45.671|     1070439|    1827.0677|
| West Bank and Gaza       | Asia      |  1962|   48.127|     1133134|    2198.9563|
| West Bank and Gaza       | Asia      |  1967|   51.631|     1142636|    2649.7150|
| West Bank and Gaza       | Asia      |  1972|   56.532|     1089572|    3133.4093|
| West Bank and Gaza       | Asia      |  1977|   60.765|     1261091|    3682.8315|
| West Bank and Gaza       | Asia      |  1982|   64.406|     1425876|    4336.0321|
| West Bank and Gaza       | Asia      |  1987|   67.046|     1691210|    5107.1974|
| West Bank and Gaza       | Asia      |  1992|   69.718|     2104779|    6017.6548|
| West Bank and Gaza       | Asia      |  1997|   71.096|     2826046|    7110.6676|
| West Bank and Gaza       | Asia      |  2002|   72.370|     3389578|    4515.4876|
| West Bank and Gaza       | Asia      |  2007|   73.422|     4018332|    3025.3498|
| Yemen, Rep.              | Asia      |  1952|   32.548|     4963829|     781.7176|
| Yemen, Rep.              | Asia      |  1957|   33.970|     5498090|     804.8305|
| Yemen, Rep.              | Asia      |  1962|   35.180|     6120081|     825.6232|
| Yemen, Rep.              | Asia      |  1967|   36.984|     6740785|     862.4421|
| Yemen, Rep.              | Asia      |  1972|   39.848|     7407075|    1265.0470|
| Yemen, Rep.              | Asia      |  1977|   44.175|     8403990|    1829.7652|
| Yemen, Rep.              | Asia      |  1982|   49.113|     9657618|    1977.5570|
| Yemen, Rep.              | Asia      |  1987|   52.922|    11219340|    1971.7415|
| Yemen, Rep.              | Asia      |  1992|   55.599|    13367997|    1879.4967|
| Yemen, Rep.              | Asia      |  1997|   58.020|    15826497|    2117.4845|
| Yemen, Rep.              | Asia      |  2002|   60.308|    18701257|    2234.8208|
| Yemen, Rep.              | Asia      |  2007|   62.698|    22211743|    2280.7699|
| Zambia                   | Africa    |  1952|   42.038|     2672000|    1147.3888|
| Zambia                   | Africa    |  1957|   44.077|     3016000|    1311.9568|
| Zambia                   | Africa    |  1962|   46.023|     3421000|    1452.7258|
| Zambia                   | Africa    |  1967|   47.768|     3900000|    1777.0773|
| Zambia                   | Africa    |  1972|   50.107|     4506497|    1773.4983|
| Zambia                   | Africa    |  1977|   51.386|     5216550|    1588.6883|
| Zambia                   | Africa    |  1982|   51.821|     6100407|    1408.6786|
| Zambia                   | Africa    |  1987|   50.821|     7272406|    1213.3151|
| Zambia                   | Africa    |  1992|   46.100|     8381163|    1210.8846|
| Zambia                   | Africa    |  1997|   40.238|     9417789|    1071.3538|
| Zambia                   | Africa    |  2002|   39.193|    10595811|    1071.6139|
| Zambia                   | Africa    |  2007|   42.384|    11746035|    1271.2116|

This result is the opposite of semi join ie it keeps only the countries that are NOT present in countries\_18. So there are 1500 rows (1704-204) and 6 columns from gapminder. The capital information from countries\_18 is not added either.

Overall, for all `join` functions, if the columns by which the join happens is not specified in the code, then it takes a column(s) with the same name(s) in both datasets.

###### Exploring match function:

``` r
matching <- match(gapminder$country, countries_18$country)

str(matching)
```

    ##  int [1:1704] NA NA NA NA NA NA NA NA NA NA ...

``` r
class(matching)
```

    ## [1] "integer"

``` r
matching
```

    ##    [1] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##   [24] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##   [47] NA NA NA NA NA NA NA NA NA NA NA NA NA NA  2  2  2  2  2  2  2  2  2
    ##   [70]  2  2  2 NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##   [93] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [116] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [139] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [162] NA NA NA NA NA NA NA 14 14 14 14 14 14 14 14 14 14 14 14 NA NA NA NA
    ##  [185] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [208] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [231] NA NA NA NA NA NA NA NA NA NA  1  1  1  1  1  1  1  1  1  1  1  1 NA
    ##  [254] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [277] NA NA NA NA NA NA NA NA NA NA NA NA  9  9  9  9  9  9  9  9  9  9  9
    ##  [300]  9 NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [323] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [346] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [369] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [392] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [415] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [438] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [461] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [484] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA 17 17
    ##  [507] 17 17 17 17 17 17 17 17 17 17 NA NA NA NA NA NA NA NA NA NA NA NA  6
    ##  [530]  6  6  6  6  6  6  6  6  6  6  6 NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [553] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [576] NA NA NA NA NA NA NA NA NA NA NA NA NA  8  8  8  8  8  8  8  8  8  8
    ##  [599]  8  8 NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [622] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [645] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [668] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [691] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [714] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [737] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [760] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [783] NA NA NA NA NA NA NA NA NA NA 10 10 10 10 10 10 10 10 10 10 10 10 NA
    ##  [806] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [829] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [852] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [875] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [898] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [921] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [944] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ##  [967] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA  5  5  5  5  5
    ##  [990]  5  5  5  5  5  5  5 NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ## [1013] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ## [1036] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ## [1059] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ## [1082] NA NA NA NA NA NA NA NA NA NA NA  3  3  3  3  3  3  3  3  3  3  3  3
    ## [1105] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ## [1128] NA NA NA NA NA NA NA NA NA NA NA NA NA  7  7  7  7  7  7  7  7  7  7
    ## [1151]  7  7 NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ## [1174] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ## [1197] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ## [1220] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ## [1243] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ## [1266] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ## [1289] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ## [1312] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ## [1335] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ## [1358] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ## [1381] NA NA NA NA NA NA NA NA NA NA NA NA 15 15 15 15 15 15 15 15 15 15 15
    ## [1404] 15 NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ## [1427] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ## [1450] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ## [1473] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ## [1496] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ## [1519] NA NA NA NA NA NA 16 16 16 16 16 16 16 16 16 16 16 16 NA NA NA NA NA
    ## [1542] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ## [1565] NA NA NA NA NA NA NA NA 11 11 11 11 11 11 11 11 11 11 11 11 NA NA NA
    ## [1588] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA  4  4
    ## [1611]  4  4  4  4  4  4  4  4  4  4 13 13 13 13 13 13 13 13 13 13 13 13 NA
    ## [1634] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ## [1657] NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA NA
    ## [1680] NA NA NA NA NA NA NA NA NA NA NA NA NA 12 12 12 12 12 12 12 12 12 12
    ## [1703] 12 12

``` r
which(gapminder == "Canada", arr.ind = T)
```

    ##       row col
    ##  [1,] 241   1
    ##  [2,] 242   1
    ##  [3,] 243   1
    ##  [4,] 244   1
    ##  [5,] 245   1
    ##  [6,] 246   1
    ##  [7,] 247   1
    ##  [8,] 248   1
    ##  [9,] 249   1
    ## [10,] 250   1
    ## [11,] 251   1
    ## [12,] 252   1

``` r
which(countries_18 == "Canada", arr.ind = T)
```

    ##      row col
    ## [1,]   1   1

``` r
matching[241:252]
```

    ##  [1] 1 1 1 1 1 1 1 1 1 1 1 1

This functions takes all the rows in gapminder from the `country` column and looks at which position the same value is found in countries\_18. The output is the list of integers (position number). If the country is not found in the countries\_18 `country` column, then NA is added. Otherwise, it gives the integer value - for example, Canada is the first in the countries\_18 so in the `matching` 241-252 values (row numbers for Canada in gapminder) it will give 1 as an output.
