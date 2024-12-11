# LeagueOfLegendsEconomyWinPrediction



# Introduction 
The game of League of Legends has grown in popularity immensely to the point where competitive leagues all around the world exist. Different updates and metas require players to stay on their toes while prioirtizing different strategies that compliment the team's gameplay in order to try and win. One common strategy is trying to get as much gold and experience by fighting and defeating monsters in the jungle areas of the map ad also killing enemies whether that be the drakes which are larger bosses or enemy towers or other elements of the game that can garner you exp and potentially serve useful later. 

The goal of this project is to take team economy stats as well as individual economy stats afer 15 minutes to try and predict whether or not the team ends up with a win or not. This is to try and determine the importance of trying to get gold for upgrades and other purposes and farming mobs as compared to actually fighting with other players from the other team. 

The dataset contains **150168 rows** and the following relevant columns that I will be using for my analysis and prediction:

- **gameid**: Unique identifier for each game.
- **position**: Role on team: top, mid, bot, jung, or support
- **result**: The outcome of the game, where a value of "Win" indicates a victory and "Loss" indicates a defeat.
- **league**: The league or division in which the game was played.
- **goldat15**: The total gold earned by the team at the 15-minute mark.
- **firstdragon**: A binary indicator (1/0) for whether the team secured the first dragon.
- **damagetochampions**: The total damage dealt to enemy champions during the game.
- **killsat15**: The number of kills the team had at the 15-minute mark.
- **earnedgold**: Total gold earned during the match.
- **golddiffat15**: The difference in gold between the two teams at the 15-minute mark.
- **earnedgoldshare**: The proportion of the team's total gold compared to the overall game.
- **xpdiffat15**: The experience difference between teams at the 15-minute mark.
- **csdiffat15**: The difference in creep score (CS) between the teams at the 15-minute mark.
- **dragons**: Total number of dragons secured during the game.
- **firsttower**: A binary indicator (1/0) for whether the team secured the first tower.

* Note: Some of the games were not longer than 15 minutes so I only used games where there were at least 15 minutes played. However, only a very small fraction of games played in the dataset were less than 15 minutes so it should not impact predicitons or analysis that much


# Data Cleaning and Exploratory Data Analysis

For each unique gameid, there were 12 rows, 5 for each team for individual stats and then 2 for team stats. For the data cleaning process, the documentation of some of the columns such as **dragons**, **firsttower**, and **firstdragon** were all team stats so they were left as NaN for individual player rows. 

As a result, for each gameid, I set the **dragons**, **firsttower**, and **firstdragon** attributes to the team stats. Additionally, some of the data collection methods for specific leagues were not as complete like the **goldat15**, **killsat15**, **golddiffat15**, **xpdiffat15**, **csdiffat15** columns. 

As a result, I did mean imputation based on the each of the positions because different positions have different roles which could on average affect the economy stats per player(i.e: a jungler who did not play as well of a game might have more gold compared to a support player who played really well). Doing this specific type of mean imputation, I was able to fill in the missing data based on the respective trends for each position rather than overgeneralizing. 

<div align="center">

| gameid               | result | position | league | goldat15 | firstdragon_team_summary | damagetochampions | killsat15 | earnedgold | golddiffat15 | earnedgoldshare | xpdiffat15 | csdiffat15 | dragons_team_summary | firsttower_team_summary |
|----------------------|--------|----------|--------|----------|--------------------------|-------------------|-----------|------------|--------------|------------------|------------|------------|-----------------------|------------------------|
| ESPORTSTMNT01_2690210 | 0      | top      | LCKC   | 5025.0   | 0.0                      | 15768.0           | 0.0       | 7164.0     | 391.0        | 0.253859         | 345.0      | 14.0       | 1.0                   | 1.0                    |
| ESPORTSTMNT01_2690210 | 0      | jng      | LCKC   | 5366.0   | 0.0                      | 11765.0           | 2.0       | 5368.0     | 541.0        | 0.190220         | -275.0     | -11.0      | 1.0                   | 1.0                    |
| ESPORTSTMNT01_2690210 | 0      | mid      | LCKC   | 5118.0   | 0.0                      | 14258.0           | 0.0       | 5945.0     | -475.0       | 0.210665         | 153.0      | 1.0        | 1.0                   | 1.0                    |
| ESPORTSTMNT01_2690210 | 0      | bot      | LCKC   | 5461.0   | 0.0                      | 11106.0           | 2.0       | 6835.0     | -793.0       | 0.242201         | -1343.0    | -34.0      | 1.0                   | 1.0                    |
| ESPORTSTMNT01_2690210 | 0      | sup      | LCKC   | 3836.0   | 0.0                      | 3663.0            | 1.0       | 2908.0     | 443.0        | 0.103054         | -497.0     | 7.0        | 1.0                   | 1.0                    |

</div>



##Univariate Data Analysis## 

<div align="center">
  <iframe
    src="plts/distribution_earnedgoldshare.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>
</div>

##Bivariate Data Analysis##
<div align="center">
  <iframe
    src="plts/golddiffat15_vs_csdiffat15.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>
</div>

