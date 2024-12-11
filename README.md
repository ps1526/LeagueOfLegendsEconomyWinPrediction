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

| position   |   ('goldat15', 0) |   ('goldat15', 1) |   ('golddiffat15', 0) |   ('golddiffat15', 1) |   ('killsat15', 0) |   ('killsat15', 1) |
|:-----------|------------------:|------------------:|----------------------:|----------------------:|-------------------:|-------------------:|
| bot        |           5307.95 |           5715.98 |              -408.029 |               408.029 |           0.767394 |           1.16237  |
| jng        |           4985.38 |           5275.55 |              -290.164 |               290.164 |           1.01417  |           1.38821  |
| mid        |           5304.77 |           5606.79 |              -302.023 |               302.023 |           0.73641  |           1.07624  |
| sup        |           3398.92 |           3585.34 |              -186.422 |               186.422 |           0.314975 |           0.426788 |
| top        |           5096.71 |           5375.99 |              -279.276 |               279.276 |           0.521953 |           0.740144 |

As we can see here from the pivot table which showcases the comparison between goldat15, golddiffat1, and killsat15 based on the end result(0 is a loss and 1 is a win) and position shows that junglers on average have higher kills compared to other groups and supports have less kills compared to other groups. Also clearly, the number of kills seems to have more of an impact compared to economy stats which is a given but we can see that overall junglers seem to have the most impact in dictating both economic stats/kills and potentially also the outcome of the match. 

On the other hand, mid and bot roles are strong in gold accumulation, while junglers stand out in kill involvement and win rates. Support roles have the lowest gold and kill metrics, but this is expected due to their utility-focused playstyle. 

This all makes sense when you consider the roles of both junglers and support characters and roles as junglers are meant to assist any of the lane players with making a push which can lead to more kills, gold, and potentially securing a win. On the other hand, support characters are meant to assist teammates and do not usually have as much damage dealing capabilities. 

# Assessment of Missingness

I do not think that there might be any columns that are NMAR because they would all be MAR to either the league column which would correlate to maybe a difference in technology and tracking systems for more competitive versus amateur leagues and then also the column for data completeness because it specifically states that if the data is complete or not for that row which would make that MAR instead of NMAR. However, I would need to dive deeper into what makes the data compelete as well as the sources of data collection during games and live matches to see if there were any further explanations for missingness in the data 

A column I wanted to test out for missingness was the *goldat15* column. After performing permutations tests to determine whether or not the data was MAR(Missing at Random) for other columns like earnedgoldshare and csdiffat15, I got that out of the columns that I tested: 'firsttower', 'firstdragon', 'dragons', 'earnedgoldshare','csdiffat15', 'killsat15', 'xpdiffat15', that all these columns except for csdiffat15 and xpdiffat15 were MAR as their p-values were all below 0.05.

For instance, here is the graph with the distributions of the goldat15 column when earnedgolshare is and is not present


 <iframe
    src="plts/goldat15mar1.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>



# Hypothesis Testing 

Null Hypothesis (H0):

The distribution of gold at 15 is the same for winning and losing matches meaning that there is no statistically significant difference in gold after 15 minutes between wins and losses

Alternative Hypothesis (H1):

There is a statistically significant difference in the distribution of gold at 15 between winning and losing matches meaning that the gold after 15 minutes values differ between wins and losses in a way that cannot be explained by random chance

Test Statistic: Difference in Means. This is a good test statistic since we are measuring a quantitative distribution of the amount of gold after 15 minutes as compared to using a statistic like Total Variation Distance which is meant for categorical distributions. 

 <iframe
    src="plts/hypothesistest.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>

  Significance Level: 0.05
  Calculated p-val using Difference in Means test statistic: 0.00

  As we can see, our calculate p-val is less than 0.05 which means that we reject our null hypothesis of the distribution of gold at 15 minutes is the same for winning and losing matches. Additionally, we can also see that there is a difference in the distribution between gold for winning and losing from our plot as well as on average winning players and teams have more gold after 15 minutes. 

# Framing a Prediction Problem

The problem that I looked at was using both individual and team economic factors at 15 minutes to try and predict if an individual was on a winning or losing team(response variable). This is a binary classification problem where 0 corresponds to a loss and 1 corresponds to a win. Specifically, I used a Logistic Regression model due to it being ideal for binary classification problems like predicting whether a player is on a winning or losing team, as it outputs probabilities and provides interpretable coefficients. 

Additionally, I used accuracy as the main scoring metric because it's a simple yet intuitive metric that measures the proportion of correct predictions and in the dataset there is no class imbalance for wins and losses, making accuracy more preferable over other evaluation metrics. 


# Baseline Model
## Model Description

I implemented a Logistic Regression classifier to predict game outcomes (`result`). The dataset includes:

- **Quantitative Features** (6): `goldat15`, `killsat15`, `golddiffat15`, `earnedgoldshare`, `xpdiffat15`, and `csdiffat15`.
- **Categorical (Nominal) Features** (5): `firstdragon_team_summary`, `dragons_team_summary`, `firsttower_team_summary`, `position`, and `league`.

### Preprocessing
- Quantitative features were standardized using `StandardScaler` for consistency in scale.
- Categorical features were one-hot encoded using `OneHotEncoder` to convert them into binary indicators.
- A `ColumnTransformer` and pipeline were used to streamline preprocessing and modeling.

---

## Model Performance

The model was evaluated on a 20% test set, achieving:
- **Accuracy**:   0.79 .
- **Confusion Matrix**

|                | Predicted Negative | Predicted Positive |
|----------------|--------------------|--------------------|
| **Actual Negative** | 9652               | 2790               |
| **Actual Positive** | 2432               | 10150              |

---

## Model Evaluation

I believe the model is **moderate** because:
- Strengths: Logistic Regression is interpretable and works well for linear relationships. The accuracy and classification metrics suggest it captures key patterns effectively.
- Weaknesses: It is not super accurate even with some engineered features. I would like at least a 81% accuracy classifier that is still able to generalize data. 




# Final Model
To improve the base model , I would want to consider adding more features specifically interaction features like the gold amount times the kills stat at 15 minutes or the earned gold times the xp difference at 15 minutes. Additionally, I would want to create aggregate features like computing the z-score of each numerical feature for each league respectively to give the model more context regarding how the individual value compares to the mean. 

## Feature Transformations

### 1. League-Specific Z-Scores
- **Purpose**: Normalize performance within league contexts
- **Rationale**: Accounts for varying competitive standards across different leagues
- **Method**: Standardize features like `goldat15`, `killsat15` using league-specific mean and standard deviation

### 2. Percentile Ranks
- **Purpose**: Provide non-linear performance representation
- **Benefits**: 
 - Robust ranking less sensitive to outliers
 - Clear view of team/player standing within league

### 3. Interaction Features
- **Interactions Created**:
 - `goldat15_killsat15_interaction`: Captures early game economic aggression
 - `earnedgold_xpdiff_interaction`: Reveals economic resource conversion to competitive advantage

### 4. Region Categorization
- **Approach**: Hierarchical league categorization
- **Tiers**: 4 levels representing competitive strength
- **Insight**: Captures implicit skill differences between leagues

### Hyperparameter Tuning
- **Method**: Grid Search CV with k=5 folds
- **Key Parameters**:
- `classifier__penalty`: `['l2']`
- `classifier__C`: `[0.001, 0.01, 0.1, 1, 10, 100]`
- `classifier__solver`: `['lbfgs']`
- `classifier__class_weight`: `[None, 'balanced']`
- `classifier__multi_class`: `['ovr', 'multinomial']`

Overall the best model was 
```python
best_params = {
    'classifier__C': 10,
    'classifier__class_weight': None,
    'classifier__multi_class': 'ovr',
    'classifier__penalty': 'l2',
    'classifier__solver': 'lbfgs'
}
```
with 'train_accuracy': 0.8346687180306905 and 'test_accuracy': 0.8363570971867008 which is better than the 0.79 that the base model was showing. 

Overall, the new model does predict slightly better than the baseline model and overall I am happy with around 83.5% accuracy due to League of Legends being a game where so many different factors can influence the outcome of the match even just in the economy attributes of the game. Additionally, here is the confusion matrix for the model:

|               | Predicted Negative | Predicted Positive |
|---------------|--------------------|--------------------|
| **Actual Negative** | 10020              | 2422               |
| **Actual Positive** | 1673               | 10909              |


# Fairness Analysis
For my fairness analysis, I wanted to see how the model would predict based on the different levels of leagues that existed in the dataset because the quality of the data is better in the higher leagues where there is more funding and money to collected data as compared to the lower ranked leagues and challenger events. 

Null Hypothesis: ur model is fair. Its precision for top tier leagues and bottom tier leagues are roughly the same, and any differences are due to random chance.

Alternative Hypothesis: Our model is unfair. Its precision for bottom tier leagues is lower than the precision for top tier leagues

Test Statistic: Difference in Means

Significance Level: 0.05

Using a permutation test and the difference in means metric, I got the p-value of 0.958 which is much higher than the significance level meaning that we fail to reject the null hypothesis. 


