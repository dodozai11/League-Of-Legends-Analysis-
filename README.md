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

#Univariate Analysis

<iframe src="assets/early_obj_score_histogram.html" width="800" height="600" frameborder="0" ></iframe>

The histogram shows that the distribution of teams' early objective score is nearly normal (consists of sum of firsttower, firstdragon, void_grubs, and killsat10). This data is well behaved and is distributed in a balanced manner among all teams.

<iframe src="assets/TeamDPM.html" width="800" height="600" frameborder="0" ></iframe>

The histogram shows that the distribution of teams' damage per minute is nearly normal. This data is well behaved and is distributed in a balanced manner among all teams.



<iframe src="assets/PermEarlyObj.html" width="800" height="600" frameborder="0" ></iframe>

