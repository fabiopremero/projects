# eSports Earnings Analysis

This project analyzes eSports earnings with archives for players and one for teams. The goal is to explore patterns in USD prize money and investigate the relationship between individual player earnings and team performance/structure.

## Analysis Questions

1. **Which games generate the highest earnings for players?**  
   - Group the players by the `game` column and sum their `totalusdprize`.
   - Visualize the results using a bar chart (with Matplotlib).

2. **How are earnings distributed among teams? Which teams are in the Top 5 and Bottom 5, and what types of games do they play?**  
   - Analyze the distribution of `TotalUSDPrize` using histograms and boxplots.
   - Identify the top 5 teams with the highest earnings and the bottom 5 teams with the lowest earnings, and display their `TeamName`, `TotalUSDPrize`, `Game`, and `Genre`.

3. **Is there a correlation between player earnings and team performance/structure?**  
   - Aggregate the earnings for both players and teams by game.
   - Merge the aggregated results and calculate the Pearson correlation between player and team earnings.

## Methodology

- **Preprocessing:**  
  - Convert text columns to lowercase when necessary.
  - Rename columns to ensure consistency across datasets.

- **Exploratory Data Analysis (EDA):**  
  - Use aggregation functions (such as `groupby` and `sum`) to summarize earnings.
  - Create visualizations (bar charts, histograms, boxplots) using Matplotlib to explore the distribution of prize money.

- **Correlation Analysis:**  
  - Group both datasets by game and compute the total earnings.
  - Merge the two results and use the `.corr()` method to calculate the Pearson correlation coefficient between aggregated player and team earnings.

## Results and Conclusions
- Games with Highest Earnings: the analysis identifies the games with the largest individual prize pools, highlighting the most attractive titles in eSports.
![BoxPlot](earning_eSports_analysis/files/teams_graphic_bar.png)

- Team Earnings Distribution: visualizations reveal that earnings are highly concentrated among a few elite teams, while most teams earn relatively lower amounts. The Top 5 and Bottom 5 teams are further examined to understand the types of games they participate in.
![BoxPlot](earning_eSports_analysis/files/boxplot.png)
![Distribution](earning_eSports_analysis/files/distribution_totalUSD.png)

- Correlation Analysis: merging the aggregated data for players and teams by game and calculating the Pearson correlation coefficient yielded a value of [insert correlation value here], suggesting [insert interpretation, e.g., a moderate/strong positive correlation between individual earnings and team performance].

```python
# 1. Group player earnings by game
players_by_game = df_players.groupby('game')['totalusdprize'].sum().reset_index()
players_by_game.rename(columns={'totalusdprize': 'players_total_prize'}, inplace=True)

# 2. Group team earnings by game
teams_by_game = df_teams.groupby('Game')['TotalUSDPrize'].sum().reset_index()
teams_by_game.rename(columns={'TotalUSDPrize': 'teams_total_prize'}, inplace=True)

# 3. Merge the two groupings based on the game name
# It may be necessary to adjust the column names (e.g., standardizing to lowercase)
merged = pd.merge(players_by_game, teams_by_game, left_on='game', right_on='Game', how='inner')
merged.drop(columns=['Game'], inplace=True)

# Calculate the Pearson correlation between the aggregated player and team earnings
correlation = merged['players_total_prize'].corr(merged['teams_total_prize'])
print("Pearson Correlation:", correlation)
print(merged)
