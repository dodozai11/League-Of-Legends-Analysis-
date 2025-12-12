# League of Legends Regional Performance Analysis

<a href="https://github.com/dodozai11/League-Of-Legends-Analysis-" class="btn btn-github" target="_blank">
  <span class="icon"></span>
  GitHub
</a>
author: Terry Su

The League of Legends Regional Performance Analysis is a case study conducted at UCSD, with the goal of exploring performance vs region
on the international stage in 2025.

# Introduction
League of Legends is a popular multiplayer game developed by Riot Games released in late 2009. In this study we will be utilizing a dataset from Oracle's Elixer,
that records data from professional matches through 2025.

In the realm of League of Legends esports, the rivalry between Eastern regions and Western regions has long captivated fans and analysts alike. This regional divide extends beyond
geography, rather it represents fundamentally different approaches to strategy, aggression, and resource management. However, in recent years the gap has increased and it seems now 
that Westtern teams are no where near capable of achieving results the way Eastern teams have. In fact, a Western team has not won Worlds since Fnatic in season 1, and that is only 
because there were no Korean or Chinese regions yet, which are the powerhouses of today. Eastern teams are often characterized by their aggressive early 
game play and decisive teamfighting, while Western teams are sometimes perceived as more methodical. Understanding these regional differences is crucial for coaches developing
international strategies, analysts predicting tournament outcomes, and fans appreciating the nuances of competitive play.

Do Eastern and Western regions demonstrate significantly different gold efficiency and early-game performance in professional League of Legends matches? Specifically, we want to 
investigate how teams from different regions convert economic advantages (gold leads) into combat success (kills and objectives), and whether early-game statistics at the 15-minute 
mark differ systematically between East and West. Using data analysis techniques, we will examine regional patterns in gold differences, objective control, and game outcomes.
Furthermore, we will develop a predictive model to forecast match winners based on 15-minute game states, while ensuring this model treats both regions fairly.

# Dataset Columns
From the Oracle Elixir Dataset, there are 118932 rows holding in-game data and descriptive statistics relating to the game. A single game takes up 12 lines in the dataset, with 10 lines to the players and two lines with an overall summary for the matched teams. For our study on performance metrics between Western and Eastern teams at international events, we will use the following columns:

- result: This column shows the winner/loser of a game, with 1 for win and 0 for loss
- teamname: name of team/organization
- league: tournament event or promotion that in which the match takes place
- firsttower: indicates the team that was able to secure the first tower takedown, with 1 for first tower take down and 0 for the opposing teamm
- firstdragon: indicates the team that was able to secure the first dragon takedown, with 1 for first dragon take down and 0 for the opposing teamm
- void_grubs: indicates the numbers of void_grubs secured by a team, ranging from 0-3
- gamelength: time a game takes to complete in seconds
- killsat10: nummber of kills a team collectively achieves in the first 10 minutes of a game
- golddiffat15: how much gold an individual player has over the opponent of the same role at 15 minutes or how much gold a team collectively has over the opposing team at 15 minutes depending on the row
- xpdiffat15: how much xp an individual player has over the opponent of the same role at 15 minutes or how much xp a team collectively has over the opposing team at 15 minutes depending on the row (xp: experience)
- csdiffat15: how much cs an individual player has over the opponent of the same role at 15 minutes or how much cs a team collectively has over the opposing team at 15 minutes depending on the row (cs: creep score, minions or jungle monseter kills)
- damagetochampions: total damage dealt to opposing player of the same role or collective team depending on row over the course of the game length

# Data Cleaning and Exploratory Data Analysis
For this case study, we will be keeping the rows result, teamname, league, firsttower, firstdragon, void_grubs, gamelength, killsat10, golddiffat15,
xpdiffat15, csdiffat15, and damagetochampions. Most of these stats are relevant as we want to focus on how often Western/Eastern teams are able to 
secure early game advantages. The study will also filter for the major 2025 leagues, that being MSI, Worlds, First Stand, and EWC, which are the big events 
where all tier 1 regions compete.

| teamname       | league | gamelength | gameid            | firsttower | firstdragon | void_grubs | killsat10 | deaths | assists | golddiffat15 | xpdiffat15 | csdiffat15 | damagetochampions | region | West/East | dpm             | early_obj_score |
|----------------|--------|------------|-------------------|------------|-------------|------------|-----------|--------|---------|--------------|------------|------------|-------------------|--------|-----------|-----------------|-----------------|
| Karmine Corp   | FST    | 1921       | LOLTMNT01_210872  | 1.0        | 0.0         | 5.0        | 0.0       | 20     | 5       | -777.0       | -500.0     | 31.0       | 59887             | LEC    | West      | 1870.494534     | 6.0             |
| Team Liquid    | FST    | 1921       | LOLTMNT01_210872  | 0.0        | 1.0         | 1.0        | 2.0       | 2      | 48      | 777.0        | 500.0      | -31.0      | 80370             | LTA    | West      | 2510.255075     | 4.0             |
| Karmine Corp   | FST    | 1400       | LOLTMNT01_210882  | 1.0        | 0.0         | 4.0        | 4.0       | 5      | 31      | 3231.0       | 2317.0     | 33.0       | 63853             | LEC    | West      | 2736.557143     | 9.0             |
| Team Liquid    | FST    | 1400       | LOLTMNT01_210882  | 0.0        | 1.0         | 2.0        | 0.0       | 20     | 7       | -3231.0      | -2317.0    | -33.0      | 40713             | LTA    | West      | 1744.842857     | 3.0             |
| Team Liquid    | FST    | 2065       | LOLTMNT01_210884  | 1.0        | 0.0         | 2.0        | 0.0       | 3      | 50      | 1990.0       | 3283.0     | 18.0       | 86972             | LTA    | West      | 2527.031477     | 3.0             |

The derived colummns region indicates team region, West/East represents whether a team is from mthe West or East, dpm represents damage to champions per minute from team, and early_obj_score is a score representing the sum of firsttower, firstdragon, voidgrubs, and killsat10.

# Univariate Analysis

<iframe src="assets/early_obj_score_histogram.html" width="800" height="600" frameborder="0" ></iframe>

The histogram shows that the distribution of teams' early objective score is nearly normal (consists of sum of firsttower, firstdragon, void_grubs, and killsat10). This data is well behaved and is distributed in a balanced manner among all teams.

<iframe src="assets/TeamDPM.html" width="800" height="600" frameborder="0" ></iframe>

The histogram shows that the distribution of teams' damage per minute is nearly normal. This data is well behaved and is distributed in a balanced manner among all teams.

# Bivariate Analysis

<iframe src="assets/regionintWR.html" width="800" height="600" frameborder="0" ></iframe>

This bar chart shows the win rates of the 5 major regions at international events. From the chart, it can be seen that LCK and LPL sport the highest winratem which follows the trend of Eastern dominance we have been seing.

# Interesting Aggregates

| region | kills | deaths | golddiffat15 | dpm | gamelength |
|--------|-------|--------|--------------|-----|------------|
| LCK    | 16.74 | 13.13 | 314.56       | 2851.70 | 1954.10   |
| LCP    | 13.33 | 15.04 | -133.91      | 2504.16 | 1911.45   |
| LEC    | 12.65 | 14.58 | -487.58      | 2514.40 | 1914.89   |
| LPL    | 14.97 | 15.36 | 365.64       | 2780.16 | 1917.61   |
| LTA    | 13.50 | 16.61 | -502.10      | 2637.45 | 1965.24   |

This Aggregate was created by using the groupby method on the region column. From the table it can be seen that LCK and LPL seem to constantly take 1st and 2nd in the metrics, also being the only regions with possitive golddiffat15. LCP is also close behind, but it seems that the Western regions LEC and LTA perform the worst

# Assignmment of Missingness

# NMAR Analysis
From the oracle Elixer dataset, it can be seen that some of the ban column values are missing. This, however, is NMAR because players can choose to not ban a champion during drafting phase. Therefore, the chance a ban value being missing is based on itself and not on another column.

# Missingness Dependency
For missingness dependency, we noticed some playername values were missing from the oracle elixer dataset, and we want to compare its chance of missingness to the other columns region and result. Firstly for the relationship between playnername and region, we developed the hypothesis pair

Null Hypothesis: Distribution of region when playername is missing is the same as the distribution of league when playername is not missing.

Alternative Hypothesis: Distribution of region when playername is missing is NOT same as the distribution of league when playername is not missing.

| region | playername_missing = False | playername_missing = True |
|--------|----------------------------|---------------------------|
| LCK    | 0.294239                   | 0.0                       |
| LCP    | 0.137860                   | 0.0                       |
| LEC    | 0.181070                   | 0.0                       |
| LPL    | 0.263374                   | 0.0                       |
| LTA    | 0.123457                   | 1.0                       |

By calculating TVD between the distribution of regions when player names are missing vs. when they're not missing, we found the observed TVD to be 0.8765 with a p_value of 0.0000. With a significane level of 0.05, we reject the null hypothesis, indicating that the chance of missingess of playername is dependent on region.

We also compared the missingness of playername to the game result column. The hypothesis pair we developed is 

Null Hypothesis: Distribution of result when playername is missing is the same as the distribution of league when playername is not missing.

Alternative Hypothesis: Distribution of result when playername is missing is NOT same as the distribution of league when playername is not missing.

| result | playername_missing = False | playername_missing = True |
|--------|----------------------------|---------------------------|
| 0      | 0.499177                   | 0.7                       |
| 1      | 0.500823                   | 0.3                       |

By calculating TVD between the distribution of results when player names are missing vs. when they're not missing, we found the observed TVD to be 0.2008 with a p_value of 0.3510. With a significane level of 0.05, since 0.05 < p_value, we fail reject the null hypothesis, indicating that the chance of missingess of playername is not dependent on region.

# Hypothesis Testing

For our study, we wanted to compare whether or not there was a difference in Western vs Eastern teams for securing early objectives. Being a reccuring sentiment by league casters or analsyt, most people belive game outcome to be determined by the early game, therefore we wanted to see if there was a significant difference in early objective control. We developed and early objective control score previously which consists of the sum of firsttower, firstdragon, void_grubs, and killsat10, which we believe to be a strong signifier of early game dominance.

Null Hypothesis: There is no difference in early objective control scores between Eastern and Western teams.

Alternative Hypothesis: There is a statistically significant difference in early objective control scores between Eastern and Western teams.

Test Statistic: Absolute mean difference in early objective control score

Signficance level: 0.05

<iframe src="assets/PermEarlyObj.html" width="800" height="600" frameborder="0" ></iframe>

With a resulting p_value of 0.0471, we reject the null hypothesis in favor of the alternative, indicating that there is a significant difference and the Eastern teams tend to have a a stronger early game peformance. This result leads us to believe that Eastern teams that manage to hold on the early game leads, can oppress and bleed out oppoents of resources, resulting in higher Eastern win rate.

# Framing a Prediction Problem

The goal is to predict whether a professional League of Legends team is from the Eastern region (LPL, LCK, LCP) or Western region (LEC, LTA) based on their in-game performance statistics measured at the 15-minute mark of a match. I chose this variable because it directly addresses the central theme of the project—analyzing regional performance differences between East and West. If regional playstyles are truly distinct, we should be able to predict a team's region from their early-game metrics, which would confirm that Eastern and Western teams exhibit measurably different approaches to the game. The evaluator I am using is accuracy and also F1 score because F1-score provides a balanced measure of both precision and recall. We have access to features such as golddiffat15, xpdiffat15, csdiffat15 (resource differences at 15 minutes), firsttower, firstdragon (early objectives that can be secured before or at 15 minutes), and league. However, we cannot use features like final kills, deaths, assists, damagetochampions, or gamelength, as these are only known after the game concludes. This ensures our model makes realistic predictions based on information available at the 15-minute checkpoint, simulating a real-world scenario where analysts might forecast regional identity mid-game.

# Baseline Model

We utilized a basic decision tree as a start for our baseline model. By incorportating the features golddiffat15, xpdiffat15, and csdiffat15, we created our decision tree model. These three quantitatve features were chosen as gold, xp, and cs leads are foundational metrics in deciding which team is ahead. After splitting the train and test data, we used GridSearch for cross validation and chose the hyperparameters criterion: gini, max_depth: 4, and min_sample_splits: 2. After fitting the model we obsverved a Training Accuracy: 0.7672
Testing Accuracy:  0.6552 and Testing F1-Score:  0.7753

These results indicate that the model has learned meaningful patterns from the early-game resource differences to distinguish between Eastern and Western teams. The gap between 76% for the training accuracy and 65% for the test accuracy indicates possible overfitting. However, the F1-score of 77.53% is notably higher than the testing accuracy, which indicates that while the model's overall predictions are 65.52% correct, it performs particularly well at balancing precision and recall. This model can be improved by upgrading to a more robust model like random forest and adding more features that indicate a strong early game.

# Final Model

In our final model, we changed our model to the random forest. We added new features such as early_objectives that consists of firsttower and firstdragon, as well as the league column, which is a categorical feature. For this categorical feature, we used OneHotEncoder to create 4 columns of the 5 major regions (dropping one). Using GridSearch for out hyperparameters, we used a max_depth of 3, min_split_sample of 20, and n_estimators of 200. After fitting the model we got 

Training Accuracy: 0.7471
Testing Accuracy:  0.7069
Testing F1-Score:  0.8132

While not a strong jump, we improved test accuracy and F1-score by about 4-5%. This indicates that the random forest model is a stronger predictor in generalized situations.

# Fairness Analysis

For our fairness analysis, we wanted to observe if there was difference in model performance in MSI and Worlds vs EWC AND FIRST STAND. This is because MSI and Worlds have signifcantly more games and is considered more prestigious. On the flip side, while EWC and First Stand still has all the tier 1 regions participating, it generally receives more critique, sometimes being called "Mickey Mouse" tournaments to downplay its importance. We want to compare accuracy in these different "tiers" of league.

Null Hypothesis: Our model is fair. Its accuracy for predicting team regions in major international tournaments (MSI, Worlds) is the same as its accuracy for minor international events (EWC, FST), and any observed differences are due to random chance.

Alternative Hypothesis: Our model is unfair. Its accuracy for predicting team regions differs significantly between major international tournaments (MSI, Worlds) and minor international events (EWC, FST).

Test Statistic: Absolute difference in accuracy between major and minor international tournaments

From our test, we found that p_value is 0.4861. With a significane level of 0.05, we fail to reject the null hypothesis, indicating that our model may be fair.


