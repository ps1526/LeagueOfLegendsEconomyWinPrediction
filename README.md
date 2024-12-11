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


| gameid               | result | position | league | goldat15 | firstdragon_team_summary | damagetochampions | killsat15 | earnedgold | golddiffat15 | earnedgoldshare | xpdiffat15 | csdiffat15 | dragons_team_summary | firsttower_team_summary |
|----------------------|--------|----------|--------|----------|--------------------------|-------------------|-----------|------------|--------------|------------------|------------|------------|-----------------------|------------------------|
| ESPORTSTMNT01_2690210 | 0      | top      | LCKC   | 5025.0   | 0.0                      | 15768.0           | 0.0       | 7164.0     | 391.0        | 0.253859         | 345.0      | 14.0       | 1.0                   | 1.0                    |
| ESPORTSTMNT01_2690210 | 0      | jng      | LCKC   | 5366.0   | 0.0                      | 11765.0           | 2.0       | 5368.0     | 541.0        | 0.190220         | -275.0     | -11.0      | 1.0                   | 1.0                    |
| ESPORTSTMNT01_2690210 | 0      | mid      | LCKC   | 5118.0   | 0.0                      | 14258.0           | 0.0       | 5945.0     | -475.0       | 0.210665         | 153.0      | 1.0        | 1.0                   | 1.0                    |
| ESPORTSTMNT01_2690210 | 0      | bot      | LCKC   | 5461.0   | 0.0                      | 11106.0           | 2.0       | 6835.0     | -793.0       | 0.242201         | -1343.0    | -34.0      | 1.0                   | 1.0                    |
| ESPORTSTMNT01_2690210 | 0      | sup      | LCKC   | 3836.0   | 0.0                      | 3663.0            | 1.0       | 2908.0     | 443.0        | 0.103054         | -497.0     | 7.0        | 1.0                   | 1.0                    |


## Univariate Data Analysis

  <iframe
    src="plts/distribution_earnedgoldshare.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>

 This plot displays the distribution of earned gold share among players. We can observe that the earned gold share has a wide spread, indicating significant variation in the values across different players. The distribution is right-skewed, meaning that most players earn a relatively smaller share of the total gold, while a smaller group earns a much larger share.

Understanding this distribution is important for analyzing the factors that contribute to disparities in gold earned by players. It can help identify which players or roles are consistently earning more gold, and offer insights into how team dynamics, strategies, or individual performance affect gold distribution throughout the match.


## Bivariate Data Analysis
  <iframe
    src="plts/golddiffat15_vs_csdiffat15.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>
The plot illustrates the relationship between creep score difference (csdiffat15) and gold difference (golddiffat15) after 15 minutes in a Summoner's Rift match. The data points are color-coded according to the match outcome: yellow for wins and blue for losses.

From the plot, we observe a positive correlation between creep score difference and gold difference, which suggests that teams with a higher creep score typically also have a greater gold advantage at the 15-minute mark. Additionally, the data points are clearly grouped, with winning teams predominantly in the upper-right quadrant and losing teams in the lower-left quadrant. This pattern highlights the importance of early-game performance as a strong predictor of the match's final outcome.

That said, there are some outliers where a team with a significant advantage in either creep score or gold difference still ends up losing the match. This could be due to other factors such as team composition, strategy, or individual player performance.

In summary, while creep score—essentially the score for killing jungle mobs—shows some correlation with gold difference after 15 minutes, the presence of these outliers indicates that there are other key factors at play in determining the final outcome.

## Grouped Data

<bound method DataFrame.to_markdown of              goldat15              golddiffat15             killsat15  \
result              0            1            0           1         0   
position                                                                
bot       5307.950034  5715.978887  -408.028852  408.028852  0.767394   
jng       4985.381275  5275.545198  -290.163923  290.163923  1.014166   
mid       5304.766245  5606.789583  -302.023338  302.023338  0.736410   
sup       3398.915177  3585.337412  -186.422235  186.422235  0.314975   
top       5096.710567  5375.986382  -279.275815  279.275815  0.521953   

                    
result           1  
position            
bot       1.162375  
jng       1.388207  
mid       1.076243  
sup       0.426788  
top       0.740144  >

As we can see here from the pivot table which showcases the comparison between goldat15, golddiffat1, and killsat15 based on the end result and position shows that junglers on average have higher kills compared to other groups and supports have less kills compared to other groups. Also clearly, the number of kills seems to have more of an impact compared to economy stats which is a given but we can see that overall junglers seem to have the most impact in dictating both economic stats/kills and potentially also the outcome of the match. 

On the other hand, mid and bot roles are strong in gold accumulation, while junglers stand out in kill involvement and win rates. Support roles have the lowest gold and kill metrics, but this is expected due to their utility-focused playstyle. 

This all makes sense when you consider the roles of both junglers and support characters and roles as junglers are meant to assist any of the lane players with making a push which can lead to more kills, gold, and potentially securing a win. On the other hand, support characters are meant to assist teammates and do not usually have as much damage dealing capabilities. 

# Assessment of Missingness


# Hypothesis Testing 

Null Hypothesis (H0):

The distribution of gold at 15 is the same for winning and losing matches meaning that there is no statistically significant difference in gold after 15 minutes between wins and losses

Alternative Hypothesis (H1):

There is a statistically significant difference in the distribution of gold at 15 between winning and losing matches meaning that the gold after 15 minutes values differ between wins and losses in a way that cannot be explained by random chance

Test Statistic: Difference in Means


# Framing a Prediction Problem


# Baseline Model


# Final Model


# Fairness Analysis

