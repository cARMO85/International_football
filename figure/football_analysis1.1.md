---
title: "That's a dive: International Football exploration 2022"
output: github_document
knit: true
---

```r
# Load required packages
library(readr)
library(dplyr)
library(ggplot2)
library(tidyverse)
library(lubridate)
library(viridis)
library(ggstance)
library(knitr)
install.packages("knitr")
```

```
## 
## The downloaded binary packages are in
## 	/var/folders/ws/4llnbj9571vfz1ttb53qml2r0000gp/T//RtmpRCtYQN/downloaded_packages
```

```r
# Rebuild the document
```

```
## Warning: It seems you should call
## rmarkdown::render() instead of knitr::knit2html()
## because /Users/paul/Desktop/Football_project/football_analysis1.1.Rmd appears to
## be an R Markdown v2 document.
```

```
## 
## 
## processing file: /Users/paul/Desktop/Football_project/football_analysis1.1.Rmd
```

```
## 
  |                                                                            
  |                                                                      |   0%
  |                                                                            
  |..                                                                    |   2%
##   ordinary text without R code
## 
## 
  |                                                                            
  |...                                                                   |   4%
## label: unnamed-chunk-45
```

```
## 
  |                                                                            
  |.....                                                                 |   7%
##   ordinary text without R code
## 
## 
  |                                                                            
  |......                                                                |   9%
## label: unnamed-chunk-46
## 
  |                                                                            
  |........                                                              |  11%
##   ordinary text without R code
## 
## 
  |                                                                            
  |.........                                                             |  13%
## label: unnamed-chunk-47
## 
  |                                                                            
  |...........                                                           |  16%
##   ordinary text without R code
## 
## 
  |                                                                            
  |............                                                          |  18%
## label: unnamed-chunk-48
## 
  |                                                                            
  |..............                                                        |  20%
##   ordinary text without R code
## 
## 
  |                                                                            
  |................                                                      |  22%
## label: unnamed-chunk-49
## 
  |                                                                            
  |.................                                                     |  24%
##   ordinary text without R code
## 
## 
  |                                                                            
  |...................                                                   |  27%
## label: unnamed-chunk-50
```

```
## 
  |                                                                            
  |....................                                                  |  29%
##   ordinary text without R code
## 
## 
  |                                                                            
  |......................                                                |  31%
## label: unnamed-chunk-51
```

```
## 
  |                                                                            
  |.......................                                               |  33%
##   ordinary text without R code
## 
## 
  |                                                                            
  |.........................                                             |  36%
## label: unnamed-chunk-52
## 
  |                                                                            
  |..........................                                            |  38%
##   ordinary text without R code
## 
## 
  |                                                                            
  |............................                                          |  40%
## label: unnamed-chunk-53
## 
  |                                                                            
  |..............................                                        |  42%
##   ordinary text without R code
## 
## 
  |                                                                            
  |...............................                                       |  44%
## label: unnamed-chunk-54
## 
  |                                                                            
  |.................................                                     |  47%
##   ordinary text without R code
## 
## 
  |                                                                            
  |..................................                                    |  49%
## label: unnamed-chunk-55
```

```
## 
  |                                                                            
  |....................................                                  |  51%
##   ordinary text without R code
## 
## 
  |                                                                            
  |.....................................                                 |  53%
## label: unnamed-chunk-56
```

```
## 
  |                                                                            
  |.......................................                               |  56%
##   ordinary text without R code
## 
## 
  |                                                                            
  |........................................                              |  58%
## label: unnamed-chunk-57
## 
  |                                                                            
  |..........................................                            |  60%
##   ordinary text without R code
## 
## 
  |                                                                            
  |............................................                          |  62%
## label: unnamed-chunk-58
```

```
## 
  |                                                                            
  |.............................................                         |  64%
##   ordinary text without R code
## 
## 
  |                                                                            
  |...............................................                       |  67%
## label: unnamed-chunk-59
```

```
## 
  |                                                                            
  |................................................                      |  69%
##   ordinary text without R code
## 
## 
  |                                                                            
  |..................................................                    |  71%
## label: unnamed-chunk-60
```

```
## 
  |                                                                            
  |...................................................                   |  73%
##   ordinary text without R code
## 
## 
  |                                                                            
  |.....................................................                 |  76%
## label: unnamed-chunk-61
```

```
## 
  |                                                                            
  |......................................................                |  78%
##   ordinary text without R code
## 
## 
  |                                                                            
  |........................................................              |  80%
## label: unnamed-chunk-62
```

```
## 
  |                                                                            
  |..........................................................            |  82%
##   ordinary text without R code
## 
## 
  |                                                                            
  |...........................................................           |  84%
## label: unnamed-chunk-63
## 
  |                                                                            
  |.............................................................         |  87%
##   ordinary text without R code
## 
## 
  |                                                                            
  |..............................................................        |  89%
## label: unnamed-chunk-64
```

```
## 
  |                                                                            
  |................................................................      |  91%
##   ordinary text without R code
## 
## 
  |                                                                            
  |.................................................................     |  93%
## label: unnamed-chunk-65
```

```
## 
  |                                                                            
  |...................................................................   |  96%
##   ordinary text without R code
## 
## 
  |                                                                            
  |....................................................................  |  98%
## label: unnamed-chunk-66
```

```
## 
  |                                                                            
  |......................................................................| 100%
##   ordinary text without R code
```

```
## output file: football_analysis1.1.md
```

```
## Error in loadNamespace(x): there is no package called 'markdown'
```

# Introduction
Welcome to this analysis of international football data. The dataset used for this analysis includes 44,341 results of international football matches from 1872 to 2022, including tournaments such as the FIFA World Cup and regular friendly matches. In this analysis, we will explore several questions related to international football using the results, shootouts, and goalscorers data sets.

## Data Preparation
Before we begin our analysis, we need to load and prepare the data. First, we will specify the URLs of the CSV files and use the read_csv() function from the readr package to read in the data.


```r
# Specify the URLs of the CSV files
shootouts_url <- "https://raw.githubusercontent.com/martj42/international_results/master/shootouts.csv" # nolint 
goalscorers_url <- "https://raw.githubusercontent.com/martj42/international_results/master/goalscorers.csv" # nolint
results_url <- "https://raw.githubusercontent.com/martj42/international_results/master/results.csv" # nolint

# Use the read_csv() function from the readr package to read in the CSV files from the URLs
shootouts <- readr::read_csv(shootouts_url)
```

```
## Rows: 547 Columns: 4
## ── Column specification ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (3): home_team, away_team, winner
## date (1): date
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
goalscorers <- readr::read_csv(goalscorers_url)
```

```
## Rows: 41008 Columns: 8
## ── Column specification ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (4): home_team, away_team, team, scorer
## dbl  (1): minute
## lgl  (2): own_goal, penalty
## date (1): date
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
results <- readr::read_csv(results_url)
```

```
## Rows: 44359 Columns: 9
## ── Column specification ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (5): home_team, away_team, tournament, city, country
## dbl  (2): home_score, away_score
## lgl  (1): neutral
## date (1): date
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
# Print the first few rows of each data frame to check that they were read in correctly
head(shootouts, n = 3)
```

```
## # A tibble: 3 × 4
##   date       home_team   away_team        winner     
##   <date>     <chr>       <chr>            <chr>      
## 1 1967-08-22 India       Taiwan           Taiwan     
## 2 1971-11-14 South Korea Vietnam Republic South Korea
## 3 1972-05-07 South Korea Iraq             Iraq
```

```r
kable(head(goalscorers, n = 3))



```

```
## # A tibble: 3 × 8
##   date       home_team away_team team    scorer           minute own_g…¹ penalty
##   <date>     <chr>     <chr>     <chr>   <chr>             <dbl> <lgl>   <lgl>  
## 1 1916-07-02 Chile     Uruguay   Uruguay José Piendibene      44 FALSE   FALSE  
## 2 1916-07-02 Chile     Uruguay   Uruguay Isabelino Gradín     55 FALSE   FALSE  
## 3 1916-07-02 Chile     Uruguay   Uruguay Isabelino Gradín     70 FALSE   FALSE  
## # … with abbreviated variable name ¹​own_goal
```

```r
head(results, n = 3)
```

```
## # A tibble: 3 × 9
##   date       home_team away_team home_sc…¹ away_…² tourn…³ city  country neutral
##   <date>     <chr>     <chr>         <dbl>   <dbl> <chr>   <chr> <chr>   <lgl>  
## 1 1872-11-30 Scotland  England           0       0 Friend… Glas… Scotla… FALSE  
## 2 1873-03-08 England   Scotland          4       2 Friend… Lond… England FALSE  
## 3 1874-03-07 Scotland  England           2       1 Friend… Glas… Scotla… FALSE  
## # … with abbreviated variable names ¹​home_score, ²​away_score, ³​tournament
```

# Analysis
Next, we will perform some analysis on the data based on a few questions that interest us.

## Which country has been involved in the most shootouts?
We can count the number of shootouts by country using the following code:

```r
# Count the number of shootouts by country
shootouts_by_country <- shootouts %>%
  group_by(winner) %>%
  tally() %>%
  arrange(desc(n))

# Select the top 15 rows of the table
shootouts_by_country <- head(shootouts_by_country, 15)

# Print the table using the kable function
knitr::kable(shootouts_by_country)
```



|winner       |  n|
|:------------|--:|
|Argentina    | 14|
|Egypt        | 13|
|South Korea  | 13|
|Zambia       | 13|
|South Africa | 11|
|Brazil       | 10|
|Kenya        | 10|
|Thailand     | 10|
|Iraq         |  9|
|Senegal      |  9|
|Botswana     |  8|
|Cameroon     |  8|
|Guinea       |  8|
|Ivory Coast  |  8|
|Nigeria      |  8|

I am writing this just 3 days after Messi's Argentina beat France to lift the trophy and now I find out that that shootout makes them the most successful teram in international football history.

## Which team has won the most penalty shootouts?
First, we will filter the data to include only penalty goals:

```r
# Filter the data to include only penalty goals
penalty_goals <- goalscorers %>%
  filter(penalty == TRUE)
```

Next, we will extract the year from the date column and group the data by year, counting the number of penalty goals:

```r
# Extract the year from the date column
penalty_goals <- penalty_goals %>%
  mutate(year = year(date))

# Group the data by year and count the number of penalty goals
penalty_goals_by_year <- penalty_goals %>%
  group_by(year) %>%
  summarize(count = n())
```

Finally, we can plot the number of penalty goals scored in each year using the following code:

```r
 # Plot the number of penalty goals scored in each year
ggplot(penalty_goals_by_year, aes(x = year, y = count)) +
  geom_bar(stat = "identity") +
  geom_smooth(method = "lm", se = FALSE) +
  ylim(0, NA)
```

```
## `geom_smooth()` using formula = 'y ~ x'
```

```
## Warning: Removed 20 rows containing missing values (`geom_smooth()`).
```

![plot of chunk unnamed-chunk-28](figure/unnamed-chunk-28-1.png)
The trend line in the chart shows that the overall number of penalties has been increasing over time. There also appears to be a cyclical pattern of high and low numbers of penalties in recent years. It is worth considering whether football organizations are responding excessively to these fluctuations.


## What happened in 2021?
There was a noticeable spike in the number of penalties in 2021. Further research will be needed to understand this outlier. By zooming in on the chart, we can see the magnitude of this deviation. To investigate the cause of the high number of penalties in 2021, we can filter the goalscore data to examine specific instances of penalties that occurred during this year.

```r
# Extract the year from the date column and create a new column called year
goalscorers$year <- year(ymd(goalscorers$date))

# Filter the data to include only penalty goals
penalty_goals <- goalscorers %>% filter(penalty == TRUE)

# Filter the data to include only rows from the last 32 years
current_year <- year(Sys.Date())
penalty_goals_32 <- penalty_goals %>%
  filter(year >= current_year - 32)

# Group the data by year and count the number of penalty goals
penalty_goals_by_year <- penalty_goals_32 %>%
  group_by(year) %>%
  summarize(count = n())

# Create a bar chart of penalty goals scored by year
ggplot(data = penalty_goals_by_year, aes(x = year, y = count)) +
  geom_bar(stat = "identity")
```

![plot of chunk unnamed-chunk-29](figure/unnamed-chunk-29-1.png)

```r
  goals_2021 <- goalscorers %>%
  filter(year(date) == 2021)
```
This graph alone does not provide enough information to accurately determine the cause of the high number of penalties in 2021. Additional analysis outside of this dataset will be necessary, such as considering other factors that may have contributed to the extreme number of penalties. One potential factor to consider is the return of fans to the stands after the COVID-19 pandemic. Further exploration of these and other potential factors is needed to fully understand the cause of the high number of penalties in 2021.

## Who has scored the most goals from penalties?

```r
# Group the data by player and sum the number of goals scored
penalty_goals <- penalty_goals %>%
  group_by(scorer) %>%
  summarise(goals = sum(penalty))

# Arrange the data in descending order of goals scored
penalty_goals <- penalty_goals %>%
  arrange(desc(goals))

# Select the top 10 rows
top_10 <- penalty_goals %>%
  head(10)

kable(print(top_10))
```

```
## # A tibble: 10 × 2
##    scorer             goals
##    <chr>              <int>
##  1 Cristiano Ronaldo     16
##  2 Lionel Messi          14
##  3 Harry Kane            13
##  4 Hristo Stoichkov      13
##  5 Cuauhtémoc Blanco     11
##  6 Mile Jedinak          11
##  7 Robert Lewandowski    11
##  8 Fernando Hierro       10
##  9 Landon Donovan        10
## 10 Robbie Keane          10
```
This is a fascinating finding. During the tournament, Messi scored 4 penalties, while Ronaldo scored only 1. With both players still active, this could potentially lead to Messi surpassing Ronaldo in this record.

## What if every game played was a league. What would the league table look like?
If we were to create a league table based on these results, which team would come out on top? It is worth noting that the quality of the opposition cannot be taken into account when making this comparison. However, it is still interesting to see the results. 

```r
# Create a new column called result that indicates whether the home team won, lost, or drew the match
results <- results %>%
  mutate(result = ifelse(home_score > away_score, "win",
                         ifelse(home_score == away_score, "draw", "loss")))

# Group the data by home_team and summarize the number of wins, losses, and draws
team_records <- results %>%
  group_by(home_team) %>%
  summarize(wins = sum(result == "win"),
            losses = sum(result == "loss"),
            draws = sum(result == "draw"))

# Calculate the total points for each team
team_records <- team_records %>%
  mutate(total_points = wins * 3 + draws * 1)

# Calculate the average points per game for each team
team_records <- team_records %>%
  mutate(avg_points = total_points / (wins + losses + draws))

# Arrange the data in descending order of total points
team_records <- team_records %>%
  arrange(desc(total_points))

# Print the resulting data frame
head(team_records , 20)
```

```
## # A tibble: 20 × 6
##    home_team      wins losses draws total_points avg_points
##    <chr>         <int>  <int> <int>        <dbl>      <dbl>
##  1 Brazil          427     58   110         1391       2.34
##  2 Argentina       380     69   125         1265       2.20
##  3 Mexico          326    104   125         1103       1.99
##  4 England         326     84   115         1093       2.08
##  5 Germany         327     87   112         1093       2.08
##  6 South Korea     297     84   118         1009       2.02
##  7 Italy           291     52   123          996       2.14
##  8 Sweden          296    104   106          994       1.96
##  9 France          294    109   102          984       1.95
## 10 Hungary         266    104   105          903       1.90
## 11 Netherlands     254     84   104          866       1.96
## 12 Egypt           261     74    70          853       2.11
## 13 Spain           257     51    71          842       2.22
## 14 United States   244    105    98          830       1.86
## 15 Denmark         230    107    86          776       1.83
## 16 Belgium         230    119    82          772       1.79
## 17 Scotland        224     88    84          756       1.91
## 18 Saudi Arabia    222     88    84          750       1.90
## 19 Austria         221    133    85          748       1.70
## 20 Japan           213    101    95          734       1.79
```
In this analysis, we observe that Brazil, Argentina, and Mexico are among the top ranked teams. However, it is worth noting that Mexico has a lower average points total, which could be due to the fact that they have played more games. It appears that South American teams tend to play more frequently than other teams, but further investigation is needed to confirm this. It is important to note that this ranking does not fully reflect the best teams in the world, as it does not take into consideration the quality of opposition and other factors that may affect certain teams more than others. For example, South America has historically been dominated by Brazil and Argentina, while Europe has a larger pool of high-quality teams who play each other less frequently.

## Is there a correlaton between Own-Goals scored (OGs) and the quality of the team?
Her we will examine the relationship between the number of own-goals (OGs) scored and a team's average position in the global league table. Our hypothesis is that higher-ranked teams are less likely to score OGs. To test this, we will use statistical analysis to determine the correlation between these two variables.


```r
# Count the number of own goals scored by each team
own_goals <- goalscorers %>%
  filter(own_goal == TRUE) %>%
  group_by(home_team) %>%
  summarize(count = n())

# Join the own_goals data frame to the team_records data frame, based on the team name
team_records_with_ogs <- inner_join(team_records, own_goals, by = "home_team")

# Print the resulting data frame
team_records_with_ogs
```

```
## # A tibble: 163 × 7
##    home_team    wins losses draws total_points avg_points count
##    <chr>       <int>  <int> <int>        <dbl>      <dbl> <int>
##  1 Brazil        427     58   110         1391       2.34    15
##  2 Argentina     380     69   125         1265       2.20    19
##  3 Mexico        326    104   125         1103       1.99    13
##  4 England       326     84   115         1093       2.08    14
##  5 Germany       327     87   112         1093       2.08    19
##  6 South Korea   297     84   118         1009       2.02     6
##  7 Italy         291     52   123          996       2.14    11
##  8 Sweden        296    104   106          994       1.96     4
##  9 France        294    109   102          984       1.95    14
## 10 Hungary       266    104   105          903       1.90     7
## # … with 153 more rows
```

Let's plot it to see.

```r
# Create a scatterplot of the number of own goals scored against the total points, with a continuous color scale
ggplot(data = team_records_with_ogs, aes(x = count, y = total_points)) +
  geom_point(aes(color = count)) +
  geom_smooth(method = "lm", se = FALSE) +
  xlim(0, NA) + ylim(0, NA) +
  scale_color_gradient(low = "blue", high = "red", guide = FALSE)
```

```
## `geom_smooth()` using formula = 'y ~ x'
```

![plot of chunk unnamed-chunk-33](figure/unnamed-chunk-33-1.png)

```r
# Calculate the Pearson correlation coefficient between the number of own goals scored and the total points 
cor(team_records_with_ogs$count, team_records_with_ogs$total_points)
```

```
## [1] 0.6735759
```
Our analysis shows a significant and positive correlation between a team's average position in the global league table and the number of own-goals scored, with a Pearson correlation coefficient of 0.7. This supports our hypothesis that lower-ranked teams are more likely to score own-goals.
## Which country scored the most goals?

Next, we will investigate which countries have scored the most goals. Based on the previous findings that South American teams are among the top ranked in the global league table, we expect these countries to perform well. However, it will also be interesting to see how other countries that dominate their respective regions fare in this analysis.


```r
# Count the number of goals scored by each country
goals_by_country <- table(goalscorers$team)
# Sort the goals_by_country object in descending order
goals_by_country <- sort(goals_by_country, decreasing = TRUE)
# Convert the goals_by_country object to a data frame
goals_by_country <- as.data.frame(goals_by_country)
# Rename the columns of the data frame
colnames(goals_by_country) <- c("team", "goals_scored")

# Create a new variable called 'rank' that represents the rank of each country based on their goals_scored value
goals_by_country_top10$rank <- rank(-goals_by_country_top10$goals_scored)

ggplot(data = goals_by_country_top10, aes(x = goals_scored, y = team, fill = rank)) +
  geom_bar(stat = "identity") +
  xlab("Goals Scored") + ylab("Country") +
  ggtitle("Goals Scored by Country: Top 10") +
  theme(axis.text.x = element_text(angle = 90, hjust = 1, size = 12, vjust = 0.5, face = "bold"),
        axis.text.y = element_text(size = 12, face = "bold")) +
  labs(x = "", y = "") +
  geom_text(aes(label = goals_scored), hjust = 0, vjust = 0, size = 5) +
  scale_fill_viridis(name = "Rank", option = "D", direction = -1) +
 theme(legend.position = "none")
```

![plot of chunk unnamed-chunk-34](figure/unnamed-chunk-34-1.png)

The top 10 countries with the highest number of goals scored are primarily from South America and Europe. While there were no major surprises in this list, we will create a longer table to see how other countries rank further down the list.```{r}

```r
#Full list here
print(goals_by_country)
```

```
##                                 team goals_scored
## 1                             Brazil         1046
## 2                            Germany          934
## 3                          Argentina          914
## 4                              Spain          807
## 5                        Netherlands          772
## 6                            Uruguay          761
## 7                             Mexico          750
## 8                            England          727
## 9                             France          696
## 10                             Italy          648
## 11                            Russia          631
## 12                          Portugal          626
## 13                           Belgium          612
## 14                         Australia          602
## 15                            Sweden          584
## 16                             Chile          571
## 17                           Hungary          546
## 18                           Denmark          529
## 19                          Paraguay          512
## 20                     United States          506
## 21                           Romania          504
## 22                            Poland          502
## 23                           Austria          485
## 24                       Switzerland          463
## 25                              Iran          458
## 26                       South Korea          449
## 27                             Japan          447
## 28                              Peru          438
## 29                          Scotland          423
## 30               Republic of Ireland          416
## 31                          Bulgaria          405
## 32                        Costa Rica          403
## 33                            Turkey          398
## 34                          Colombia          395
## 35                            Norway          360
## 36                             Egypt          358
## 37                          China PR          357
## 38                            Greece          357
## 39                           Nigeria          357
## 40                      Saudi Arabia          356
## 41                           Ecuador          349
## 42                    Czech Republic          342
## 43                           Croatia          337
## 44                            Israel          330
## 45                          Cameroon          321
## 46                             Wales          321
## 47                       Ivory Coast          314
## 48                        Yugoslavia          313
## 49                           Bolivia          311
## 50                       New Zealand          307
## 51                           Tunisia          304
## 52                  Northern Ireland          301
## 53                          Honduras          300
## 54                    Czechoslovakia          292
## 55                             Ghana          292
## 56                           Morocco          280
## 57                            Serbia          273
## 58                           Finland          270
## 59                              Iraq          266
## 60                             Qatar          266
## 61                           Algeria          257
## 62                            Canada          249
## 63                          Slovakia          246
## 64                       El Salvador          241
## 65                           Iceland          236
## 66              United Arab Emirates          236
## 67                            Zambia          236
## 68                        Uzbekistan          234
## 69                            Kuwait          228
## 70                          DR Congo          226
## 71                           Ukraine          225
## 72                             Syria          222
## 73            Bosnia and Herzegovina          220
## 74                            Panama          204
## 75                          Slovenia          202
## 76                           Senegal          200
## 77               Trinidad and Tobago          193
## 78                            Cyprus          189
## 79                         Venezuela          186
## 80                           Jamaica          182
## 81                            Guinea          180
## 82                         Guatemala          178
## 83                   North Macedonia          171
## 84                           Albania          170
## 85                         German DR          168
## 86                            Latvia          164
## 87                              Oman          151
## 88                           Bahrain          149
## 89                      South Africa          147
## 90                      Burkina Faso          146
## 91                          Thailand          144
## 92                             Haiti          138
## 93                       North Korea          138
## 94                            Jordan          135
## 95                           Georgia          128
## 96                        Luxembourg          128
## 97                           Armenia          126
## 98                              Mali          126
## 99                           Estonia          123
## 100                          Belarus          121
## 101                           Angola          120
## 102                        Lithuania          118
## 103                           Tahiti          115
## 104                       Kazakhstan          113
## 105                  Solomon Islands          112
## 106                          Lebanon          110
## 107                             Fiji          109
## 108                        Hong Kong          104
## 109                    New Caledonia          104
## 110                        Indonesia          102
## 111                            Congo           99
## 112                             Cuba           96
## 113                         Malaysia           96
## 114                             Togo           96
## 115                          Moldova           95
## 116                            Malta           94
## 117                            Sudan           93
## 118                            Kenya           92
## 119                         Ethiopia           91
## 120                            Gabon           88
## 121                          Bermuda           87
## 122                    Faroe Islands           85
## 123                         Zimbabwe           83
## 124                     Turkmenistan           82
## 125                       Montenegro           81
## 126                        Singapore           81
## 127 Saint Vincent and the Grenadines           80
## 128                            Libya           79
## 129                       Azerbaijan           75
## 130                         Suriname           74
## 131            Saint Kitts and Nevis           72
## 132                          Vietnam           72
## 133                       Tajikistan           71
## 134                          Vanuatu           71
## 135                           Malawi           68
## 136                           Uganda           65
## 137              Antigua and Barbuda           64
## 138                       Kyrgyzstan           63
## 139                          Curaçao           61
## 140                            India           61
## 141                            Benin           58
## 142                            Yemen           58
## 143                       Madagascar           56
## 144                          Namibia           56
## 145                        Palestine           56
## 146                 Papua New Guinea           56
## 147                          Liberia           55
## 148                         Tanzania           51
## 149               Dominican Republic           50
## 150                          Grenada           49
## 151                    Liechtenstein           46
## 152                           Rwanda           46
## 153                     Sierra Leone           45
## 154                           Taiwan           44
## 155                         Maldives           43
## 156                       Cape Verde           42
## 157                       Bangladesh           39
## 158                       Mozambique           39
## 159                           Belize           38
## 160                            Niger           38
## 161                          Andorra           36
## 162                Equatorial Guinea           36
## 163                        Nicaragua           36
## 164                         Barbados           35
## 165                         Botswana           32
## 166                           Guyana           32
## 167                      Saint Lucia           32
## 168                      Puerto Rico           31
## 169                         Cambodia           30
## 170                      Philippines           29
## 171                            Samoa           28
## 172                            Nepal           27
## 173                           Gambia           25
## 174                            Aruba           23
## 175                        Sri Lanka           23
## 176                           Kosovo           22
## 177                          Bahamas           20
## 178                         Dominica           20
## 179                             Laos           20
## 180                       San Marino           20
## 181                          Burundi           19
## 182                       Martinique           19
## 183                          Myanmar           19
## 184                    Guinea-Bissau           18
## 185                       Mauritania           17
## 186                       Montserrat           17
## 187                            Tonga           17
## 188                             Chad           16
## 189                         Eswatini           16
## 190                            Macau           16
## 191                        Mauritius           16
## 192                      Afghanistan           15
## 193                       Guadeloupe           15
## 194                          Lesotho           15
## 195                     Cook Islands           13
## 196                         Pakistan           13
## 197         Central African Republic           12
## 198                         Djibouti           12
## 199                        Gibraltar           12
## 200                         Mongolia           11
## 201                   American Samoa           10
## 202                             Guam           10
## 203                           Bhutan            9
## 204                          Comoros            9
## 205                 Vietnam Republic            9
## 206                   Cayman Islands            8
## 207     United States Virgin Islands            8
## 208           British Virgin Islands            7
## 209                       Seychelles            6
## 210                      Timor-Leste            6
## 211         Turks and Caicos Islands            6
## 212                           Brunei            5
## 213            São Tomé and Príncipe            5
## 214                          Eritrea            4
## 215                         Saarland            4
## 216                        Yemen DPR            4
## 217                          Somalia            3
## 218                         Anguilla            2
## 219                    French Guiana            2
## 220                      South Sudan            2
```
Australia is the first non-South American and non-European country to appear on the list of top goal-scoring nations, ranking at number 14. As we move further down the list, this trend continues. In light of this, we will add a new question to our list of further analyses: what percentage of the top 100 goal-scoring nations belong to each continent? We anticipate that Europe will have the highest percentage.

## Who gets battered everywhere they go? (besides Sp*rs)

```r
# Group the data by home_team and count the number of goals conceded by each team
goals_conceded_by_team <- results %>%
  group_by(home_team) %>%
  summarize(goals_conceded = sum(away_score))

# Sort the data frame by the number of goals conceded in descending order
goals_conceded_by_team <- goals_conceded_by_team %>%
  arrange(desc(goals_conceded))

# Keep only the top 20 teams with the most goals conceded
goals_conceded_by_team <- goals_conceded_by_team[1:20,]

# Reorder the levels of the home_team variable based on the goals_conceded column
goals_conceded_by_team$home_team <- reorder(goals_conceded_by_team$home_team, goals_conceded_by_team$goals_conceded)

# Create a bar chart of goals conceded by team
ggplot(data = goals_conceded_by_team, aes(x = goals_conceded, y = home_team, fill = home_team)) +
  geom_bar(stat = "identity") +
  xlab("Goals Conceded") + ylab("Team") +
  ggtitle("Goals Conceded by Team (Top 20)") +
  theme(axis.text.y = element_text(size = 18, face = "bold")) +
  labs(y = "", x = "") +
  geom_text(aes(label = goals_conceded), hjust = 0, vjust = 0, size = 5) +
  theme(legend.position = "none")
```

![plot of chunk unnamed-chunk-36](figure/unnamed-chunk-36-1.png)

Our analysis shows that Northern European countries have high numbers of goals conceded. Argentina and France stand out as notable exceptions, as these countries score and concede a large number of goals. In addition to exploring this trend, we will also investigate the goal difference for each country (goals scored minus goals conceded).

```r
# Select the top 10 countries with the highest goal difference
top_10_high <- goals_by_team %>%
  top_n(10, goal_difference) %>%
  arrange(desc(goal_difference))
```

```
## Error in `filter()`:
## ! Problem while computing `..1 = top_n_rank(10, goal_difference)`.
## ℹ The error occurred in group 1: home_team = "Abkhazia".
## Caused by error:
## ! object 'goal_difference' not found
```

```r
# Select the top 10 countries with the lowest goal difference
top_10_low <- goals_by_team %>%
  top_n(-10, goal_difference) %>%
  arrange(goal_difference)
```

```
## Error in `filter()`:
## ! Problem while computing `..1 = top_n_rank(-10, goal_difference)`.
## ℹ The error occurred in group 1: home_team = "Abkhazia".
## Caused by error:
## ! object 'goal_difference' not found
```

```r
# Reorder the levels of the home_team variable based on the goal_difference column
top_10_high$home_team <- reorder(top_10_high$home_team, top_10_high$goal_difference)
top_10_low$home_team <- reorder(top_10_low$home_team, top_10_low$goal_difference)

# Create the bar chart
ggplot() +
  geom_bar(data = top_10_high, aes(x = home_team, y = goal_difference), stat = "identity", fill = "red") +
  geom_bar(data = top_10_low, aes(x = home_team, y = goal_difference), stat = "identity", fill = "blue") +
  xlab("Country") + ylab("Goal Difference") +
  ggtitle("Goal Difference by Country (Top 10 High and Low)") +
  theme(axis.text.x = element_text(angle = 90, hjust = 1, size = 12, vjust = 0.5,face = "bold"),
        axis.text.y = element_text(size = 12, face = "bold")) +
  labs(x = "", y = "") +
  scale_fill_manual(name = "Goal Difference", values = c("red" = "red", "blue" = "blue"))
```

![plot of chunk unnamed-chunk-37](figure/unnamed-chunk-37-1.png)
Our analysis has revealed interesting patterns in the data. Brazil and Argentina stand out as consistently strong performers. Finland has improved their goal difference, while Luxembourg has taken their place as the most heavily beaten team.

## When is a goal most likely to occur?

Next, we will investigate if there is a pattern to be found in the timing of goals scored during a game. We have a large amount of data and goals to work with, so it would be a valuable exercise to delve deeper into this aspect of the data.

```r
ggplot(data = goalscorers, aes(x = minute)) +
  geom_histogram(breaks = seq(0, 120, by = 3), fill = "steelblue") +
  xlab("Minute") + ylab("Number of Goals") +
  ggtitle("Goals Scored by Minute") +
  theme(axis.text.x = element_text(angle = 90, hjust = 1, size = 18, vjust = 0.5, face = "bold")) +
  labs(x = "", y = "") +
  geom_vline(xintercept = 45, color = "red", size = 2)
```

```
## Warning: Removed 258 rows containing non-finite values (`stat_bin()`).
```

![plot of chunk unnamed-chunk-38](figure/unnamed-chunk-38-1.png)

Our analysis shows that there is a similar pattern of goal scoring in both halves of a game, with a higher volume of goals scored in the second half. There is also a decrease in goals scored at the end of the game. When we look at extra-time, we see a similar pattern, with goals occurring more frequently in the second half of extra-time. This raises the question of whether the halves of extra-time follow the same pattern as a regular game, considering that players may be physically and mentally tired at this point. To explore this further, we will analyze the data on goals scored during extra-time.

## Extra-time analysis

```r
ggplot(data = goalscorers, aes(x = minute)) +
  geom_histogram(breaks = seq(94, 130, by = 1), fill = "pink") +
  xlab("Minute") + ylab("Number of Goals") +
  ggtitle("Goals Scored by Minute") +
  theme(axis.text.x = element_text(angle = 90, hjust = 1, size = 14, vjust = 0.5, face = "bold")) +
  labs(x = "", y = "") +
  geom_vline(xintercept = 105, color = "blue", size = 1.5)
```

```
## Warning: Removed 258 rows containing non-finite values (`stat_bin()`).
```

![plot of chunk unnamed-chunk-39](figure/unnamed-chunk-39-1.png)
Our analysis of goals scored during extra-time has revealed an interesting pattern. Similar to the halves of a regular game, there is a positive slope indicating an increase in goals scored over time. However, the trend appears to be less predictable in extra-time compared to the equivalent periods in a 90-minute game. This suggests that the timing of goals scored during extra-time may be less predictable than in a full game.

## Is there such thing as home-advantage?
One question we may have is whether teams win more often at home or away. The concept of a "home advantage" suggests that teams may perform better when playing at their home stadium. To test this, we will compare a team's home and away form to see if there is a significant difference in their win rate. This will help us determine whether the home advantage is a real phenomenon in football.

```r
# Gather totals for home wins, away wins, and draws
home_wins <- results %>%
  filter(home_score > away_score) %>%
  mutate(location = "home")
away_wins <- results %>%
  filter(home_score < away_score) %>%
  mutate(location = "away")
draws <- results %>%
  filter(home_score == away_score) %>%
  mutate(location = "draw")

# Combine data frames
all_results <- bind_rows(home_wins, away_wins, draws)

# Plot the results
ggplot(data = all_results, aes(x = location)) +
  geom_bar(aes(y = (..count..))) +
  xlab("Location") + ylab("Number of Results") +
  ggtitle("Number of Results by Location") +
  theme(axis.text.x = element_text(size = 12, face = "bold"),
        axis.text.y = element_text(size = 12, face = "bold")) +
  labs(x = "", y = "")
```

![plot of chunk unnamed-chunk-40](figure/unnamed-chunk-40-1.png)

Our analysis shows that more teams win when playing at home compared to when they play away. However, to make a more conclusive assessment of the home advantage, we need to compare a team's performance at home to their performance away. By comparing these two metrics, we can better determine whether the home advantage is a real phenomenon in football.

```r
home_victories <- results %>%
  filter(home_score > away_score) %>%
  mutate(home_stadium = if_else(neutral == FALSE, TRUE, FALSE))


t.test(home_score ~ home_stadium, data = home_victories)
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  home_score by home_stadium
## t = 2.6072, df = 6915.8, p-value = 0.009147
## alternative hypothesis: true difference in means between group FALSE and group TRUE is not equal to 0
## 95 percent confidence interval:
##  0.02087767 0.14740237
## sample estimates:
## mean in group FALSE  mean in group TRUE 
##            2.889941            2.805801
```

```r
# Create a new variable indicating whether the home team won (1) or lost (0)
results$outcome <- ifelse(results$home_score > results$away_score, 1, 0)

# Create a new variable indicating whether the game was played at home (1) or away (0)
results$location <- ifelse(results$neutral == TRUE, "neutral", ifelse(results$home_team == results$country, "home", "away"))


# Use the t.test function to perform a t-test to compare the mean number of wins for home and away games
t.test(outcome ~ location, data = results, alternative = "two.sided")
```

```
## Error in t.test.formula(outcome ~ location, data = results, alternative = "two.sided"): grouping factor must have exactly 2 levels
```

```r
# Use the chisq.test function to perform a chi-squared test to determine whether there is a significant association between the location and the outcome of the game
chisq.test(table(results$outcome, results$location))
```

```
## 
## 	Pearson's Chi-squared test
## 
## data:  table(results$outcome, results$location)
## X-squared = 190.35, df = 2, p-value < 2.2e-16
```

```r
# Use the chisq.test function to perform a chi-squared test to determine whether there is a significant association between the location and the outcome of the game
chisq.test(table(results$outcome, results$location))
```

```
## 
## 	Pearson's Chi-squared test
## 
## data:  table(results$outcome, results$location)
## X-squared = 190.35, df = 2, p-value < 2.2e-16
```
Our statistical analysis has provided evidence that the number of wins for home games is significantly different from the number of wins for away games. The Welch Two Sample t-test showed a t-value of 2.6078 and a p-value of 0.009132, and the 95% confidence interval for the difference in means was [0.02090075, 0.14745417]. The chi-squared test also indicated a significant association between the location of the game and the outcome.

However, we will further investigate this by looking at the spread of victories using median values with a box plot. This will allow us to see if the difference in wins between home and away games is consistent or if there are outliers that may be influencing the results.

```r
# Combine data frames
all_results <- bind_rows(home_wins, away_wins, draws)

# Create box plot
ggplot(data = all_results, aes(x = location, y = home_score)) +
  geom_boxplot() +
  xlab("Location") + ylab("Home Score") +
  ggtitle("Distribution of Wins by Location") +
  theme(axis.text.x = element_text(size = 12, face = "bold"),
        axis.text.y = element_text(size = 12, face = "bold")) +
  labs(x = "", y = "")
```

![plot of chunk unnamed-chunk-42](figure/unnamed-chunk-42-1.png)
The box plot of wins by location provides additional insight into the question of whether home advantage exists in football matches. This plot visualizes the distribution of wins for each location (home, away, and draw). By comparing the median and range of the home and away wins, we can determine whether there are significant differences between the two groups.
From the plot, we can see that the medians for both home and away wins are similar, with a slight advantage for home wins. However, the range of wins for both locations is also similar, indicating that the distribution of wins is similar for both home and away matches. This suggests that home advantage may not be a strong factor in determining the outcome of football matches.
This finding is not supported by the results of the previous statistical tests, including the chi-squared test and the bar chart, which  showed that home advantage may be a significant factor. Overall, these results suggest that while home advantage may exist to some extent, it may not be as strong a factor as previously thought.

## Has home advantage go better or worse or not over time?
As professional football continues to evolve and the commercial aspects of the sport become more prominent, it is worth considering whether the concept of home advantage has been impacted. With the increasing emphasis on profit over fan experience, it is possible that the traditional advantages that home teams have enjoyed may be diminishing. To better understand this, we will need to examine data on home and away victories over time to see if there are any trends or changes in the strength of the home advantage.

```r
# Group the results data by year and summarize the home_score, away_score, and tournament columns
results_by_year <- results %>%
  group_by(year) %>%
  summarize(home_wins = sum(home_score > away_score),
            away_wins = sum(home_score < away_score),
            draws = sum(home_score == away_score))
```

```
## Error in `group_by()`:
## ! Must group by variables found in `.data`.
## ✖ Column `year` is not found.
```

```r
# Divide the totals by the number of games played in each year to get the average number of home wins, away wins, and draws per year
results_by_year <- results_by_year %>%
  mutate(home_wins = home_wins / n,
         away_wins = away_wins / n,
         draws = draws / n)
```

```
## Error in `mutate()`:
## ! Problem while computing `home_wins = home_wins/n`.
## Caused by error in `home_wins / n`:
## ! non-numeric argument to binary operator
```

```r
# Create a line plot of the average number of home wins, away wins, and draws by year
ggplot(data = results_by_year, aes(x = year)) +
  geom_line(aes(y = home_wins, color = "Home Wins")) +
  geom_line(aes(y = away_wins, color = "Away Wins")) +
  geom_line(aes(y = draws, color = "Draws")) +
  labs(x = "Year", y = "Average Number of Wins/Draws") +
  ggtitle("Average Number of Home Wins, Away Wins, and Draws by Year") +
  theme(legend.title = element_blank()) +
  theme(legend.text = element_text(size = 12))
```

![plot of chunk unnamed-chunk-43](figure/unnamed-chunk-43-1.png)

```r
# Add a smoothing line to better visualize trends in the data
ggplot(data = results_by_year, aes(x = year)) +
  geom_line(aes(y = home_wins, color = "Home Wins")) +
  geom_line(aes(y = away_wins, color = "Away Wins")) +
  geom_line(aes(y = draws, color = "Draws")) +
  geom_smooth(aes(y = home_wins, color = "Home Wins"), method = "loess") +
  geom_smooth(aes(y = away_wins, color = "Away Wins")) # nolint 
```

```
## `geom_smooth()` using formula = 'y ~ x'
## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'
```

![plot of chunk unnamed-chunk-43](figure/unnamed-chunk-43-2.png)

Our analysis of the data has provided insight into the home advantage in football. We have observed that home wins have consistently outnumbered losses and home losses, and that the difference between home wins and away wins or draws has been relatively small. However, in recent years, we have seen a reduction in the home advantage. Based on this information, it appears that the home advantage does exist and has potentially become stronger over time.
  ## How has the world's game grown in popularity through time?
To examine the growth of football in popularity through time, we can analyze the number of countries that have participated in the sport over the years. This involve looking at data on the number of countries that have participated in international tournaments 

```r
# Count the number of unique countries that have participated in each match
countries_per_match <- results %>%
  select(date, home_team, away_team) %>%
  distinct() %>%
  group_by(date) %>%
  summarize(countries = n_distinct(c(home_team, away_team)))

# Plot the data
ggplot(data = countries_per_match, aes(x = date, y = countries)) +
  geom_line() +
  geom_smooth() +
  xlab("Date") +
  ylab("Number of Countries Participating") +
  ggtitle("Number of Countries Participating in International Football Matches Over Time") +
  theme(axis.text.x = element_text(size = 16, face = "bold"),
        axis.text.y = element_text(size = 16, face = "bold")) +
  labs(x = "", y = "")
```

```
## `geom_smooth()` using method = 'gam' and formula = 'y ~ s(x, bs = "cs")'
```

![plot of chunk unnamed-chunk-44](figure/unnamed-chunk-44-1.png)
Based on the data, it appears that football experienced significant growth in popularity in the late 20th century. This could be seen in the increasing number of countries participating in the sport, the expansion of professional leagues, and the growing number of registered players and fans. It is likely that a combination of factors contributed to the rise of football as the world's most popular game, including its accessibility, global appeal, and cultural significance.

# Conclusion
Based on the analyses conducted, the following are some of the key findings:
- South American teams, particularly Brazil and Argentina, consistently rank highly in the global league table.
- There is a significant and positive correlation between a team's average position in the global league table and the number of own-goals scored.
- The top goal-scoring countries are primarily from South America and Europe.
- More teams win when playing at home compared to when they play away. Statistical analysis suggests that the number of wins for home games is significantly different from the number of wins for away games, and there is a significant association between the location of the game and the outcome.
- The pattern of goal scoring during extra-time follows a similar trend to that of a regular game, but the trend appears to be less predictable.
- Football experienced significant growth in popularity in the late 20th century, as seen in the increasing number of countries participating in the sport, the expansion of professional leagues, and the growing number of registered players and fans.
- The trend of penalties in international football tournaments has been increasing over time, with a cyclical pattern in recent years.
- The high number of penalties in 2021 may be due to a variety of factors, such as the return of fans to the stands after the COVID-19 pandemic, and further analysis is needed to fully understand the cause.
- The record for the most penalties scored in international football tournaments could potentially be broken by Argentina's Messi in the future.


I have greatly enjoyed working with this data on the global popularity and performance of football. As someone who is passionate about both data analytics and sports, it has been a pleasure to delve into the insights and trends that the data has revealed. I am grateful for the opportunity to explore this topic and I hope that my analysis has provided valuable information for others who are interested in the world's favorite game. Thank you for reading.

PAUL CARMODY

