# A League of Legends Analysis: From a Team Contribution Perspective

League of Legends Analysis: From a Team Contribution Perspective is UCSD Data Science project aimed at looking at competitive League of Legends data at various stages of analysis. The types of analysis included within this project includes: exploratory analysis, hypothesis testing, prediction modeling, and fairness analysis of the prediction models.

The main goal of this project is to find the significance of "visionscore" and its influence on match outcomes other game aspects.

Author: Edgar Cisneros Barron

## Introduction

### About League of Legends and the Data

League of Legends is a multiplayer online battle arena game (or MOBA) created by Riot Games. The game encompasses one of the worlds largest esports scenes where millions of players watch or compete in esport teams. 

The dataset that we have worked on is a data set that was gathered and created by Oracle's Elixir. The dataset encompasses competitive esports match data from the year 2022.

In League of Legends, the concept of "vision score" is a score derived from contributing "vision" to your team. The team consists of 5 players, and in the arena, the only information available to your team from the start is 

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

