# A League of Legends Analysis: From a Team Contribution Perspective

League of Legends Analysis: From a Team Contribution Perspective is UCSD Data Science project aimed at looking at competitive League of Legends data at various stages of analysis. The types of analysis included within this project includes: exploratory analysis, hypothesis testing, prediction modeling, and fairness analysis of the prediction models.

The main goal of this project is to find the significance of "Vision Score" and its influence on match outcomes other game aspects.

Author: Edgar Cisneros Barron

## Introduction

### About League of Legends and the Data

League of Legends is a multiplayer online battle arena game (or MOBA) created by Riot Games. The game encompasses one of the worlds largest esports scenes where millions of players watch or compete in esport teams. 

The dataset that we have worked on is a data set that was gathered and created by Oracle's Elixir. The dataset encompasses competitive esports match data from the year 2022.

In League of Legends, the concept of "vision score" is a score derived from contributing "vision" to your team. The team consists of 5 players, and in the arena, the only information available to your team from the start is the starting area where your team starts. A player can provide a team more 'vision' by exploring the map and temporarily revealing a portion of the map for your team on the minimap. PLayers can further contribute by leaving Wards, or totem-liek objects that stand in place maintaining an area revealed for the team until destroyed by enemy players. This action would restrict the information available on the minimap as revealed parts of the minimap displays enemy positions.

### Columns Used
Oracle Elixir's dataset spans 161 columns of data from 150,180 rows of individual player and team suammary match data. Out of this large dataset, we picked out these columns to assess player and team performance.

- `gameid` : This column identifies individual competitive matches that the row belongs to.

- `teamname` : This columns serves to identify what esports team this row belongs to.

- `win` : This column displays whether the row of the team or player won the match indicated by a 1 as a victory, or 0 as a loss. For the sake of legibility, we renamed this column frmo its original name 'result'.

- `league` : This column identifies which Esports League tournamnet the match took place in

- `side` : This column serves to identify which team the player or team is on for the match. A team is always either on 'Blue' or 'Red' side

- `kills` : This column denotes how many kills the team or individual player obtained during the match. A 'kill' is defined as a taking down an opposing player character and dealing the final blow.  This is not for Non-Player Character kills (NPCs such Monsters and Minions)

- `deaths` : This columns denotes how many times a particular player, or team died during a match

- `assists` : This column denotes how many assists a player or team obtained. An "assist" is rewarded when a player assists a teammate in dwindling an enemy player's health points to zero, where the teammate deals the final killing blow.

- `monsterkills` : This column shows the number of Monster NPCs a player or team has killed. Neutral monster non-player characters roam the map for both teams to claim kills against before the enemy team does for rewards mid-match

- `minionkills` : This column shows the number of enemy team Minions a player or team has killed during the duration of the match. Non-player characters named Minions are automatically sent to the enemy's side of the arena to attack the enemy team's base

- `visionscore` : This column shows the total Vision Score contributed during the duration of the match. Vision Score is a game stat displaying how much 'vision' a teammate provides the team by revealing section of the map temporarily by travelign around the map and revealing all of the area the player sees on their screen onto the team's minimap of the arena. 

- `wardsplaced` : This column shows the number of Wards placed by a player or team. Wards are objects a player can place that will reveal a portion of the map surrounding the Ward while it is active. This contributes to additional Vision Score.

- `wardskilled` : This column shows the number of enemy Wards destroyed by a player or team. This results in cutting off the enemy team's visibility of the arena via the minimap.

- `firstblood` : This column indicates whether a team obtained the first kill of the match. Obtaining the first kill of the match obtains the in-game even of First Blood.

- `team kpm` : This column has the game stat indicating KPM for the entire team. KPM is an acronym for Kills per Minute.

- `totalgold` : This column shows the total number of Gold obtained during the match. Gold is obtained passively during a match, but through methods such as the use of items, abilities, player kills, monster kills, and other player activity, players can increase their Gold generation. This makes it a good indicator of player/team activity

## Data Cleaning and Exploratory Analysis
Since we will not use the whole dataset, we only kept the necessary columns.

We kept the columns that were mentioned above with the addition of the `datacompleteness` column that was used to assist in cleaning up the dataset by identifying which rows of data have missing values. 

This column made it easy to filter out and identify rows and columns that contained missing values. By doing this, it was found that the `minionkills` and `firstblood` contained missing values. 

Both columns had the following solution to fill in their missing values:

- `minionkills` : The dataset followed a strict format of 10 player rows followed by 2 team summary rows. The rows of team summary stats contained missign values. This made it easy to sum the player stats into their respective team summary rows. 

- `firstblood` : This column was luckily paired with another column named `firstbloodkill` which showed which player obtained the First Blood kill. This column generally mimicked the same stat as our `firstblood` column, so we filled in the missing values using the `firstbloodkill`column's value.

Among the dataset, there was a match that had zero data for `minionkills` and other columns. There was nothing usable to impute the missing values, so the game was dropped from the dataset entirely.

The dataset did contain both individual player data and team summary data. For the purposes of our use, we mainly focused on team summary data and filtered out the individual player data.

The head of the cleaned dataframe:

|    | teamname          | league   | side   | position   |   kills |   deaths |   assists |   monsterkills |   minionkills |   wardsplaced |   wardskilled |   visionscore | win   |   firstblood |   team kpm |   totalgold |
|---:|:------------------|:---------|:-------|:-----------|--------:|---------:|----------:|---------------:|--------------:|--------------:|--------------:|--------------:|:------|-------------:|-----------:|------------:|
|  0 | BRION Challengers | LCKC     | Blue   | top        |       2 |        3 |         2 |             11 |           220 |             8 |             6 |            26 | False |            0 |     0.3152 |       10934 |
|  1 | BRION Challengers | LCKC     | Blue   | jng        |       2 |        5 |         6 |            115 |            33 |             6 |            18 |            48 | False |            1 |     0.3152 |        9138 |
|  2 | BRION Challengers | LCKC     | Blue   | mid        |       2 |        2 |         3 |             16 |           177 |            19 |             7 |            29 | False |            0 |     0.3152 |        9715 |
|  3 | BRION Challengers | LCKC     | Blue   | bot        |       2 |        4 |         2 |             18 |           208 |            12 |             6 |            25 | False |            1 |     0.3152 |       10605 |
|  4 | BRION Challengers | LCKC     | Blue   | sup        |       1 |        5 |         6 |              0 |            42 |            29 |            14 |            69 | False |            1 |     0.3152 |        6678 |

### Univariate Analysis
After performing univariate analysis on the Vision Score, we found that the distribution of of Vision Scores for all teams to be somewhat Normally shaped. It is mildly skewed to the right. 

<iframe
  src="assets/visionscore_uni.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

This distribution indicates that our vision scores are not unusual for the typical competitive match. 

### Bivariate Analysis
We then dove deeper and analyzed whether teams with High Vision Scores had lower or higher win rates compared to Low Vision Scoring teams

<iframe
  src="assets/visionscore_win_bi.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

As seen above, teams with higher visino scores had a higher win rate overall. Teams with lower visionscore lost more often.

### Interesting Aggregates
Here was an interesting aggregate that was found.


| win   |    kills |   deaths |   assists |   monsterkills |   minionkills |   wardsplaced |   wardskilled |   visionscore |   firstblood |   team kpm |   totalgold |
|:------|---------:|---------:|----------:|---------------:|--------------:|--------------:|--------------:|--------------:|-------------:|-----------:|------------:|
| False |  9.36855 | 19.6298  |   20.076  |        180.894 |       791.478 |       95.2623 |       41.4919 |       211.571 |     0.390043 |   0.291051 |     51925.4 |
| True  | 19.6096  |  9.40619 |   44.5897 |        215.889 |       812.268 |       98.8539 |       45.7536 |       238.074 |     0.608359 |   0.641814 |     61908.2 |

First, we filtered out the players to keep only team summary data, and then grouped by wins and took the means. Overall, as expected, every stat had improvements for thw winning team, and most notably, higher visionscores but only by around 30 points.

## Assessment of Missingness

### NMAR Analysis
Among all the columns, there are 5 `ban` columns named `ban1` - `ban5` indicating players banning player playable Champion characters from benig player by the enemy team. Whether or no a champion is ban for that match is up to the players as the can choose to ban or not to ban any Champions. Fof this reason, it leads us ot believe that the `ban` columns have NMAR missingness.

### Missingness Dependency

Using team summary data, we will check the missingness dependency of the `minionkills` column over other columns within this section.

We first used tested if the missingness dependency of the `minionkills` column depended on if the match was a playoff game using the column `playoffs` from the original dataset.

The `playoffs` column indicates whether or not a match was a playoff game with 1 indicating yes, and 0 indicating it was not.

We ran a Permutation test by shuffling the `playoffs` column and creating a column showing whether or not the `minionkills` value was missing within the original dataset.

We had the following Hypotheses:

**Null:** The distribution of `minionkills` being missing on `playoffs` is the same as the distribution of `minionkills` notbeing missing on `playoffs`.

**Alternative:** The distribution of `minionkills` being missing on `playoffs` is **NOT** the same as the distribution of `minionkills` notbeing missing on `playoffs`.

<iframe
  src="assets/playoffs_permutation_missing.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

With a p-value of **0.0** and significance level of 0.05, we reject the null. `minionkills` is dependent on `playoffs`.

After this, we ran a second Permutation Test by checking the missingness dependency of `minionkills` on `side`. 

The same thing as before was done, shuffle the team `side` column and check for dependency.

**Null:** The distribution of `minionkills` being missing on `side` is the same as the distribution of `minionkills` not being missing on `side`.

**Alternative:** The distribution of `minionkills` being missing on `side` is **NOT** the same as the distribution of `minionkills` notbeing missing on `side`.

<iframe
  src="assets/team_side_permutation_missing.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

With a p-value of 1.0 and significance level of 0.05, we accept the null. `minionkills` isindependent of team `side`.

## Hypothesis Testing
For our Hypothesis Test, we wanted to see how much Vision Score impacted team Monster Kill counts using Difference of Means as our test statistic.

**Null:** The distribution of monster kills with a high visionscore is the same as the
distribution of kills for teams with low visionscore.

**Alt Hypothesis:** The distribution of monster kills for teams with high visionscore is different than those teams with low visionscore.

We used a significance level of 0.05.

<iframe
  src="assets/mon_kill_hypo.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The column `monsterkills` demonstrates a normal distribution, and we found that the hypothesis test resulted in a p-value of **0.0**. We reject the null.

This suggests that Vision Score seems to not contribute to Monster kills during a game. Monster kills may have depend on other factors that do not rely on team information as monsters remain within certain areas of the arena.

## Framing a Prediction Problem

After our Hypothesis Test, we found that visionscore seems to have little or no impact on NPC kills such as monsters. But as we saw in our Bivariate Analysis, it seems to have contribution on whether or not a team wins or loses. Visionscore is only one of many factors that wins a game, so we can also make use of kills, assists, deaths, team kpm, and firstblood columns to help predict whether a team won or lost. 

During a game, team statistics are always available to view mid-game where you can see player `kills`, `deaths`, and `assists` among other metrics for both teams. It makes it possible to make predictions while a game is being played at the time of prediction.

**Prediction Problem:** Can we predict whether a team won or lost according to team stat summaries?

This sort of problem is a Classification problem. A team will be predicted to either win or lose. Only two options, so this is a specifically a Binary Classification problem trying to predict the `win` column. For this case, we can use a Logistical Regression model.

Since we will be using a Classification model, F-1 Score and Accuracy will be our accuracy metrics as we can assess the model's Accuracy by seeing how many True Positives it achieved while also assessing the balance between Recall and Precision using the F-1 Score.

Fortunately, the column is already in a 1 or 0 format, so there is not need to apply a One-Hot Encoder.

To start employing machine learning techniques, we can try to use basic stats: `kills`, `deaths`, and `assists`, as the predictor for a team winning or losing a game for our baseline model. The data will be given a 5 fold split of 20% test data and 80% training data.

We will later add `firstblood`, `team kpm`, and `totalgold` to further improve our baseline model where firstblood is our only categorical column among the others. It also avoids the need to apply a One-Hot Encoder as it is alreawdy in a 1 or 0 format. These are also in game metrics or events that are tracked or trackable mid-game at the time prediction.

## Baseline Model
For our baseline model, the columns being used are all quantitative columns on a binary prediction, which led us to believe that we can use a Logistical Regression classifcation model for this prediction.

Our Baseline Model achieved a surprising **0.9556 Accuracy** and a similar **0.9560 F-1 Score**.

This suggests that the model was able to correctly predict 95.56% of the match outcomes correctly while also maintaining high Recall and Precision close to 1. We can try to manage near perfect predictions in the Final Model using more mid-game scoreboard metrics.

## Final Model
For the Final Model, we added the columns: `firstblood`, `team kpm`, and `totalgold`.
These columns further assesses team performance by showing increased number of kills and objective completions during matches. 

Originally, a Random Forest Classifier was fit to see for improvement first. It acheived very similar F-1 Score and Accuracy as our Baseline Model, so we returned back to the Logistical Regression and achieved a nearly 1% increase on both metrics.

Our Final Model achieved: **0.9646 Accuracy** and **0.9650 F-1 Score**

Among the other data available, this may be highes accuracy we can achieve. Other remaining scoreboard stats like `minionkills` and `monsterkills` are normally distributed, so they are likely to give minimal impact on our model's predictions.

## Fairness Analysis
To assess the fairness of our model, we tracked the differences in F-1 Scores among all leagues in the `league` column. Since we used a classification model, we can use the F-1 Score to assess the Precision and Recall of our model among all esports leagues.

We can take all observed F-1 Scores per league, and run a Permutation Test to see if any league suffered from decreased F-1 Score unfairly compred to the rest.

This would make our group 'X' be our current League being compared, and our Group Y would be all other leagues.

In other words: Does our model perform worse for league 'X' than it does for all other leagues 'Y'.

**Null:** Our Model is Fair. The precision of this model across all League of Legends competitive leagues is roughly the same across all leagues.

**Alt:** Our model is unfair. One or more leagues has precision lower than the other leagues.

With a significance level of 0.05, we accept the null with a p-value of **0.1146** and deem our model is fair

