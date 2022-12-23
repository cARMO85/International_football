---
title: That's a dive:International Football exploration 2022
output: github_document
knit: true
author: Paul Carmody
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
library(rmarkdown)
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
kable(head(shootouts, n = 3))
```



|date       |home_team   |away_team        |winner      |
|:----------|:-----------|:----------------|:-----------|
|1967-08-22 |India       |Taiwan           |Taiwan      |
|1971-11-14 |South Korea |Vietnam Republic |South Korea |
|1972-05-07 |South Korea |Iraq             |Iraq        |

```r
kable(head(goalscorers, n = 3))
```



|date       |home_team |away_team |team    |scorer           | minute|own_goal |penalty |
|:----------|:---------|:---------|:-------|:----------------|------:|:--------|:-------|
|1916-07-02 |Chile     |Uruguay   |Uruguay |José Piendibene  |     44|FALSE    |FALSE   |
|1916-07-02 |Chile     |Uruguay   |Uruguay |Isabelino Gradín |     55|FALSE    |FALSE   |
|1916-07-02 |Chile     |Uruguay   |Uruguay |Isabelino Gradín |     70|FALSE    |FALSE   |

```r
kable(head(results, n = 3))
```



|date       |home_team |away_team | home_score| away_score|tournament |city    |country  |neutral |
|:----------|:---------|:---------|----------:|----------:|:----------|:-------|:--------|:-------|
|1872-11-30 |Scotland  |England   |          0|          0|Friendly   |Glasgow |Scotland |FALSE   |
|1873-03-08 |England   |Scotland  |          4|          2|Friendly   |London  |England  |FALSE   |
|1874-03-07 |Scotland  |England   |          2|          1|Friendly   |Glasgow |Scotland |FALSE   |

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
kable(shootouts_by_country)
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

## Has the volume of penalties changed over time??
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
  ylim(0, NA) +
  ylab ("Count of penalties")

  
```

![image description](https://github.com/cARMO85/International_football/blob/master/football_analysisv2_files/figure-html/unnamed-chunk-6-1.png)


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
  geom_bar(stat = "identity")+
  ylab ("Count of penalties")
```
```r
  goals_2021 <- goalscorers %>%
  filter(year(date) == 2021)
```
![image description](https://github.com/cARMO85/International_football/blob/master/football_analysisv2_files/figure-html/unnamed-chunk-7-1.png)


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



|scorer             | goals|
|:------------------|-----:|
|Cristiano Ronaldo  |    16|
|Lionel Messi       |    14|
|Harry Kane         |    13|
|Hristo Stoichkov   |    13|
|Cuauhtémoc Blanco  |    11|
|Mile Jedinak       |    11|
|Robert Lewandowski |    11|
|Fernando Hierro    |    10|
|Landon Donovan     |    10|
|Robbie Keane       |    10|
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
kable(head(team_records , 20))
```



|home_team     | wins| losses| draws| total_points| avg_points|
|:-------------|----:|------:|-----:|------------:|----------:|
|Brazil        |  427|     58|   110|         1391|   2.337815|
|Argentina     |  380|     69|   125|         1265|   2.203833|
|Mexico        |  326|    104|   125|         1103|   1.987387|
|England       |  326|     84|   115|         1093|   2.081905|
|Germany       |  327|     87|   112|         1093|   2.077947|
|South Korea   |  297|     84|   118|         1009|   2.022044|
|Italy         |  291|     52|   123|          996|   2.137339|
|Sweden        |  296|    104|   106|          994|   1.964427|
|France        |  294|    109|   102|          984|   1.948515|
|Hungary       |  266|    104|   105|          903|   1.901053|
|Netherlands   |  254|     84|   104|          866|   1.959276|
|Egypt         |  261|     74|    70|          853|   2.106173|
|Spain         |  257|     51|    71|          842|   2.221636|
|United States |  244|    105|    98|          830|   1.856823|
|Denmark       |  230|    107|    86|          776|   1.834515|
|Belgium       |  230|    119|    82|          772|   1.791183|
|Scotland      |  224|     88|    84|          756|   1.909091|
|Saudi Arabia  |  222|     88|    84|          750|   1.903553|
|Austria       |  221|    133|    85|          748|   1.703872|
|Japan         |  213|    101|    95|          734|   1.794621|




In this analysis, we observe that Brazil, Argentina, and Mexico are among the top ranked teams. However, it is worth noting that Mexico has a lower average points total, which could be due to the fact that they have played more games. It appears that South American teams tend to play more frequently than other teams, but further investigation is needed to confirm this. It is important to note that this ranking does not fully reflect the best teams in the world, as it does not take into consideration the quality of opposition and other factors that may affect certain teams more than others. For example, South America has historically been dominated by Brazil and Argentina, while Europe has a larger pool of high-quality teams who play each other less frequently.

## Is there a correlaton between Own-Goals scored (OGs) and the quality of the team?
Here we will examine the relationship between the number of own-goals (OGs) scored and a team's average position in the global league table. Our hypothesis is that higher-ranked teams are less likely to score OGs. To test this, we will use statistical analysis to determine the correlation between these two variables.


```r
# Count the number of own goals scored by each team
own_goals <- goalscorers %>%
  filter(own_goal == TRUE) %>%
  group_by(home_team) %>%
  summarize(count = n())

# Join the own_goals data frame to the team_records data frame, based on the team name
team_records_with_ogs <- inner_join(team_records, own_goals, by = "home_team")

# Print the resulting data frame
kable(head(team_records_with_ogs, n = 50))
```



|home_team            | wins| losses| draws| total_points| avg_points| count|
|:--------------------|----:|------:|-----:|------------:|----------:|-----:|
|Brazil               |  427|     58|   110|         1391|   2.337815|    15|
|Argentina            |  380|     69|   125|         1265|   2.203833|    19|
|Mexico               |  326|    104|   125|         1103|   1.987387|    13|
|England              |  326|     84|   115|         1093|   2.081905|    14|
|Germany              |  327|     87|   112|         1093|   2.077947|    19|
|South Korea          |  297|     84|   118|         1009|   2.022044|     6|
|Italy                |  291|     52|   123|          996|   2.137339|    11|
|Sweden               |  296|    104|   106|          994|   1.964427|     4|
|France               |  294|    109|   102|          984|   1.948515|    14|
|Hungary              |  266|    104|   105|          903|   1.901053|     7|
|Netherlands          |  254|     84|   104|          866|   1.959276|     7|
|Egypt                |  261|     74|    70|          853|   2.106173|     8|
|Spain                |  257|     51|    71|          842|   2.221636|    13|
|United States        |  244|    105|    98|          830|   1.856823|    12|
|Denmark              |  230|    107|    86|          776|   1.834515|     1|
|Belgium              |  230|    119|    82|          772|   1.791183|    12|
|Scotland             |  224|     88|    84|          756|   1.909091|    10|
|Saudi Arabia         |  222|     88|    84|          750|   1.903553|     3|
|Austria              |  221|    133|    85|          748|   1.703872|    11|
|Japan                |  213|    101|    95|          734|   1.794621|     9|
|Poland               |  209|     96|   100|          727|   1.795062|    11|
|Chile                |  213|    115|    77|          716|   1.767901|    10|
|Uruguay              |  199|     62|    98|          695|   1.935933|     6|
|Portugal             |  201|     59|    85|          688|   1.994203|    12|
|China PR             |  198|     69|    78|          672|   1.947826|     3|
|Costa Rica           |  197|     53|    78|          669|   2.039634|     4|
|Romania              |  195|     73|    75|          660|   1.924198|     6|
|Switzerland          |  188|    141|    95|          659|   1.554245|     8|
|Morocco              |  194|     47|    72|          654|   2.089457|     6|
|Russia               |  190|     50|    71|          641|   2.061093|    18|
|Ghana                |  187|     41|    74|          635|   2.102649|     4|
|Tunisia              |  182|     59|    83|          629|   1.941358|     1|
|Nigeria              |  185|     36|    69|          624|   2.151724|     3|
|Zambia               |  177|     52|    93|          624|   1.937888|     1|
|Iran                 |  186|     45|    63|          621|   2.112245|     3|
|Ivory Coast          |  183|     36|    67|          616|   2.153846|     4|
|Norway               |  171|    141|   103|          616|   1.484337|     7|
|Trinidad and Tobago  |  182|     73|    67|          613|   1.903727|     5|
|Thailand             |  175|     93|    78|          603|   1.742775|     4|
|Algeria              |  174|     61|    75|          597|   1.925806|     9|
|Australia            |  174|     73|    58|          580|   1.901639|     8|
|Kuwait               |  162|     96|    89|          575|   1.657061|     7|
|Indonesia            |  170|    111|    64|          574|   1.663768|     2|
|Qatar                |  164|     93|    80|          572|   1.697329|     3|
|Iraq                 |  162|     47|    78|          564|   1.965157|     1|
|Malawi               |  152|     93|   104|          560|   1.604585|     2|
|Cameroon             |  161|     47|    73|          556|   1.978648|     3|
|United Arab Emirates |  157|     84|    76|          547|   1.725552|     4|
|Republic of Ireland  |  149|     85|    91|          538|   1.655385|    16|
|Bulgaria             |  153|     88|    69|          528|   1.703226|     7|

Let's plot it to see.

```r
# Create a scatterplot of the number of own goals scored against the total points, with a continuous color scale
ggplot(data = team_records_with_ogs, aes(x = count, y = total_points)) +
  geom_point(aes(color = count)) +
  geom_smooth(method = "lm", se = FALSE) +
  xlim(0, NA) + ylim(0, NA) +
  scale_color_gradient(low = "blue", high = "red", guide = FALSE) +
  ggtitle("My Chart Title") +
  theme(plot.title = element_text(size = 20)) +
  xlab("Count of own goals scored") 
```



![image description](https://github.com/cARMO85/International_football/blob/master/football_analysisv2_files/figure-html/unnamed-chunk-11-1.png)

```r
# Calculate the Pearson correlation coefficient between the number of own goals scored and the total points 
correlation <- cor(team_records_with_ogs$count, team_records_with_ogs$total_points)
correlation
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

# Use the head() function to show only the top 40 countries
kable(head(goals_by_country, n = 40))

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

![image description](https://github.com/cARMO85/International_football/blob/master/football_analysisv2_files/figure-html/unnamed-chunk-12-1.png)

The top 10 countries with the highest number of goals scored are primarily from South America and Europe. While there were no major surprises in this list, we will create a longer table to see how other countries rank further down the list. Also please see table below for more details.



|team                             | goals_scored|
|:--------------------------------|------------:|
|Brazil                           |         1046|
|Germany                          |          934|
|Argentina                        |          914|
|Spain                            |          807|
|Netherlands                      |          772|
|Uruguay                          |          761|
|Mexico                           |          750|
|England                          |          727|
|France                           |          696|
|Italy                            |          648|
|Russia                           |          631|
|Portugal                         |          626|
|Belgium                          |          612|
|Australia                        |          602|
|Sweden                           |          584|
|Chile                            |          571|
|Hungary                          |          546|
|Denmark                          |          529|
|Paraguay                         |          512|
|United States                    |          506|
|Romania                          |          504|
|Poland                           |          502|
|Austria                          |          485|
|Switzerland                      |          463|
|Iran                             |          458|
|South Korea                      |          449|
|Japan                            |          447|
|Peru                             |          438|
|Scotland                         |          423|
|Republic of Ireland              |          416|
|Bulgaria                         |          405|
|Costa Rica                       |          403|
|Turkey                           |          398|
|Colombia                         |          395|
|Norway                           |          360|
|Egypt                            |          358|
|China PR                         |          357|
|Greece                           |          357|
|Nigeria                          |          357|
|Saudi Arabia                     |          356|
|Ecuador                          |          349|
|Czech Republic                   |          342|
|Croatia                          |          337|
|Israel                           |          330|
|Cameroon                         |          321|
|Wales                            |          321|
|Ivory Coast                      |          314|
|Yugoslavia                       |          313|
|Bolivia                          |          311|
|New Zealand                      |          307|
|Tunisia                          |          304|
|Northern Ireland                 |          301|
|Honduras                         |          300|
|Czechoslovakia                   |          292|
|Ghana                            |          292|
|Morocco                          |          280|
|Serbia                           |          273|
|Finland                          |          270|
|Iraq                             |          266|
|Qatar                            |          266|
|Algeria                          |          257|
|Canada                           |          249|
|Slovakia                         |          246|
|El Salvador                      |          241|
|Iceland                          |          236|
|United Arab Emirates             |          236|
|Zambia                           |          236|
|Uzbekistan                       |          234|
|Kuwait                           |          228|
|DR Congo                         |          226|
|Ukraine                          |          225|
|Syria                            |          222|
|Bosnia and Herzegovina           |          220|
|Panama                           |          204|
|Slovenia                         |          202|
|Senegal                          |          200|
|Trinidad and Tobago              |          193|
|Cyprus                           |          189|
|Venezuela                        |          186|
|Jamaica                          |          182|
|Guinea                           |          180|
|Guatemala                        |          178|
|North Macedonia                  |          171|
|Albania                          |          170|
|German DR                        |          168|
|Latvia                           |          164|
|Oman                             |          151|
|Bahrain                          |          149|
|South Africa                     |          147|
|Burkina Faso                     |          146|
|Thailand                         |          144|
|Haiti                            |          138|
|North Korea                      |          138|
|Jordan                           |          135|
|Georgia                          |          128|
|Luxembourg                       |          128|
|Armenia                          |          126|
|Mali                             |          126|
|Estonia                          |          123|
|Belarus                          |          121|
|Angola                           |          120|
|Lithuania                        |          118|
|Tahiti                           |          115|
|Kazakhstan                       |          113|
|Solomon Islands                  |          112|
|Lebanon                          |          110|
|Fiji                             |          109|
|Hong Kong                        |          104|
|New Caledonia                    |          104|
|Indonesia                        |          102|
|Congo                            |           99|
|Cuba                             |           96|
|Malaysia                         |           96|
|Togo                             |           96|
|Moldova                          |           95|
|Malta                            |           94|
|Sudan                            |           93|
|Kenya                            |           92|
|Ethiopia                         |           91|
|Gabon                            |           88|
|Bermuda                          |           87|
|Faroe Islands                    |           85|
|Zimbabwe                         |           83|
|Turkmenistan                     |           82|
|Montenegro                       |           81|
|Singapore                        |           81|
|Saint Vincent and the Grenadines |           80|
|Libya                            |           79|
|Azerbaijan                       |           75|
|Suriname                         |           74|
|Saint Kitts and Nevis            |           72|
|Vietnam                          |           72|
|Tajikistan                       |           71|
|Vanuatu                          |           71|
|Malawi                           |           68|
|Uganda                           |           65|
|Antigua and Barbuda              |           64|
|Kyrgyzstan                       |           63|
|Curaçao                          |           61|
|India                            |           61|
|Benin                            |           58|
|Yemen                            |           58|
|Madagascar                       |           56|
|Namibia                          |           56|
|Palestine                        |           56|
|Papua New Guinea                 |           56|
|Liberia                          |           55|
|Tanzania                         |           51|
|Dominican Republic               |           50|
|Grenada                          |           49|
|Liechtenstein                    |           46|
|Rwanda                           |           46|
|Sierra Leone                     |           45|
|Taiwan                           |           44|
|Maldives                         |           43|
|Cape Verde                       |           42|
|Bangladesh                       |           39|
|Mozambique                       |           39|
|Belize                           |           38|
|Niger                            |           38|
|Andorra                          |           36|
|Equatorial Guinea                |           36|
|Nicaragua                        |           36|
|Barbados                         |           35|
|Botswana                         |           32|
|Guyana                           |           32|
|Saint Lucia                      |           32|
|Puerto Rico                      |           31|
|Cambodia                         |           30|
|Philippines                      |           29|
|Samoa                            |           28|
|Nepal                            |           27|
|Gambia                           |           25|
|Aruba                            |           23|
|Sri Lanka                        |           23|
|Kosovo                           |           22|
|Bahamas                          |           20|
|Dominica                         |           20|
|Laos                             |           20|
|San Marino                       |           20|
|Burundi                          |           19|
|Martinique                       |           19|
|Myanmar                          |           19|
|Guinea-Bissau                    |           18|
|Mauritania                       |           17|
|Montserrat                       |           17|
|Tonga                            |           17|
|Chad                             |           16|
|Eswatini                         |           16|
|Macau                            |           16|
|Mauritius                        |           16|
|Afghanistan                      |           15|
|Guadeloupe                       |           15|
|Lesotho                          |           15|
|Cook Islands                     |           13|
|Pakistan                         |           13|
|Central African Republic         |           12|
|Djibouti                         |           12|
|Gibraltar                        |           12|
|Mongolia                         |           11|
|American Samoa                   |           10|
|Guam                             |           10|
|Bhutan                           |            9|
|Comoros                          |            9|
|Vietnam Republic                 |            9|
|Cayman Islands                   |            8|
|United States Virgin Islands     |            8|
|British Virgin Islands           |            7|
|Seychelles                       |            6|
|Timor-Leste                      |            6|
|Turks and Caicos Islands         |            6|
|Brunei                           |            5|
|São Tomé and Príncipe            |            5|
|Eritrea                          |            4|
|Saarland                         |            4|
|Yemen DPR                        |            4|
|Somalia                          |            3|
|Anguilla                         |            2|
|French Guiana                    |            2|
|South Sudan                      |            2|



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
  theme(axis.text.y = element_text(size = 12, face = "bold")) +
  labs(y = "", x = "") +
  geom_text(aes(label = goals_conceded), hjust = 0, vjust = 0, size = 5) +
  theme(legend.position = "none")

```

![image description](https://github.com/cARMO85/International_football/blob/master/football_analysisv2_files/figure-html/unnamed-chunk-14-1.png)

Our analysis shows that Northern European countries have high numbers of goals conceded. Argentina and France stand out as notable exceptions, as these countries score and concede a large number of goals. In addition to exploring this trend, we will also investigate the goal difference for each country (goals scored minus goals conceded).

```r
# Select the top 10 countries with the highest goal difference
top_10_high <- goals_by_team %>%
  top_n(10, goal_difference) %>%
  arrange(desc(goal_difference))
```

```r
# Select the top 10 countries with the lowest goal difference
top_10_low <- goals_by_team %>%
  top_n(-10, goal_difference) %>%
  arrange(goal_difference)
 
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

![image description](https://github.com/cARMO85/International_football/blob/master/figure/unnamed-chunk-411-1.png)



Our analysis has revealed interesting patterns in the data. Brazil and Argentina stand out as consistently strong performers. Finland has improved their goal difference, while Luxembourg has taken their place as the most heavily beaten team.

## When is a goal most likely to occur?

Next, we will investigate if there is a pattern to be found in the timing of goals scored during a game. We have a large amount of data and goals to work with, so it would be a valuable exercise to delve deeper into this aspect of the data.

```r
ggplot(data = goalscorers, aes(x = minute)) +
  geom_histogram(breaks = seq(0, 120, by = 3), fill = "steelblue") +
  xlab("Minute") + ylab("Number of Goals") +
  ggtitle("Goals Scored by Minute") +
  theme(axis.text = element_text(size = 16)) +
  labs(x = "Minute in game", y = "Total no. of goals scored") +
  geom_vline(xintercept = 45, color = "red", size = 2) +
  scale_x_continuous(breaks = seq(0, 120, by = 5))

```

```
## Warning: Removed 258 rows containing non-finite values (`stat_bin()`).
```

![image description](https://github.com/cARMO85/International_football/blob/master/figure/unnamed-chunk-448-1.png)


Our analysis shows that there is a similar pattern of goal scoring in both halves of a game, with a higher volume of goals scored in the second half. There is also a decrease in goals scored at the end of the game. When we look at extra-time, we see a similar pattern, with goals occurring more frequently in the second half of extra-time. This raises the question of whether the halves of extra-time follow the same pattern as a regular game, considering that players may be physically and mentally tired at this point. To explore this further, we will analyze the data on goals scored during extra-time.

## Extra-time analysis

```r
ggplot(data = goalscorers, aes(x = minute)) +
  geom_histogram(breaks = seq(94, 130, by = 1), fill = "pink") +
  xlab("Minute") + ylab("Number of Goals") +
  ggtitle("Goals Scored by Minute") +
  theme(axis.text = element_text(size = 16)) +
  labs(x = "Minute in game", y = "Total no. og goals scored") +
  geom_vline(xintercept = 105, color = "blue", size = 1.5) +
  scale_x_continuous(breaks = seq(94, 130, by = 5))
```

```
## Warning: Removed 258 rows containing non-finite values (`stat_bin()`).
```

![image description](https://github.com/cARMO85/International_football/blob/master/figure/unnamed-chunk-449-1.png)


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
  labs(x = "Location of game played", y = "Total number of results")
```

![image description](https://github.com/cARMO85/International_football/blob/master/figure/unnamed-chunk-450-1.png)


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
```

```r
# Perform chi-squared test
chisqtest <- chisq.test(table(results$outcome, results$location))
chisqtest
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

![image description](https://github.com/cARMO85/International_football/blob/master/figure/unnamed-chunk-453-1.png)


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

```r
# Add a smoothing line to better visualize trends in the data
ggplot(data = results_by_year, aes(x = year)) +
  geom_line(aes(y = home_wins, color = "Home Wins")) +
  geom_line(aes(y = away_wins, color = "Away Wins")) +
  geom_line(aes(y = draws, color = "Draws")) +
  geom_smooth(aes(y = home_wins, color = "Home Wins"), method = "loess") +
  geom_smooth(aes(y = away_wins, color = "Away Wins")) # nolint 
```




![image description](https://github.com/cARMO85/International_football/blob/master/figure/unnamed-chunk-454-2.png)


Our analysis of the data has provided insight into the home advantage in football. We have observed that home wins have consistently outnumbered losses and home losses, and that the difference between home wins and away wins or draws has been relatively small. However, in recent years, we have seen a reduction in the home advantage. Based on this information, it appears that the home advantage does exist and has potentially become stronger over time.

## How has the world's game grown in popularity through time?
To examine the growth of football in popularity through time, we can analyze the number of countries that have participated in the sport over the years. This involve looking at data on the number of countries that have participated in international tournaments 

```r
# Count the number of unique countries that have participated in each match
ggplot(data = countries_per_match, aes(x = date, y = countries)) +
  geom_line(color = "red", size = 2) +
  geom_smooth(color = "black") +
  xlab("Date") +
  ylab("Number of Countries Participating") +
  ggtitle("Number of Countries Participating in International Football Matches Over Time") +
  theme(axis.text.x = element_text(size = 16, face = "bold"),
        axis.text.y = element_text(size = 16, face = "bold")) +
  labs(x = "", y = "") +
  scale_color_manual(name = "Line Type", values = c("red" = "red", "smooth" = "black"))
```

![image description](https://github.com/cARMO85/International_football/blob/master/figure/unnamed-chunk-455-1.png)






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