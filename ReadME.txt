Messi vs. Ronaldo: Who Was the More Effective Player?

A data analysis project comparing Lionel Messi and Cristiano Ronaldo's per-season goal contributions (goals + assists) to determine whether one player was statistically more effective than the other — not just who had bigger raw numbers.

Project Goal

Raw goal/assist totals are misleading on their own — a player with more games played will naturally accumulate more stats. This project builds a fair, normalized effectiveness metric, visualizes it over time, and then tests whether the observed difference between the two players is statistically meaningful or just normal season-to-season variation.

Data Source

Messi_vs_Ronaldo Stat.xlsx — season-by-season games, goals, and assists for both players across ~19 matched seasons (2004/2005 through 2022/2023).

Seasons with 0 games played (pre-debut / non-playing years) were excluded from the analysis, since they would distort averages.

Methodology

1. Reshaping The raw data is "wide" (Messi and Ronaldo's stats side by side in the same row). It was reshaped into "long" format — one row per player, per season — which is the standard structure for comparison, plotting, and SQL querying.

2. Effectiveness metric Since minutes played wasn't available in this dataset, effectiveness is measured as goal contributions per game:

Goal_Contributions = Goals + Assists
GC_per_Game = Goal_Contributions / Games

This normalizes for playing time so a 60-game season and a 30-game season can be fairly compared.

3. Visualization A season-by-season line chart of GC_per_Game for both players, to visually compare peaks, consistency, and trends over their careers.

4. Statistical test A paired t-test was used (not just comparing raw averages) because the data is naturally paired — each season gives one value for Messi and one for Ronaldo, so the test can account for season-level factors that might affect both players similarly (e.g. league era, rule changes).

t-statistic: measures the size of the gap between the two players' averages, scaled against how much their numbers naturally vary season to season. A bigger t means the gap looks more like a real, consistent pattern rather than random noise.
p-value: the probability of seeing a gap this large purely by chance, if the two players were actually equally effective. A p-value below 0.05 is the conventional threshold for calling a result "statistically significant" — meaning the gap is unlikely to be random.

5. SQL layer The cleaned dataset was also loaded into a local SQLite database (soccer_stats.db) to demonstrate the same analysis using SQL — including GROUP BY aggregations and window functions (RANK(), LAG()) for per-player season rankings and year-over-year change.

Results
Player	Avg. GC per Game
Messi	1.088
Ronaldo	0.990

Paired t-test:

t-statistic = 1.752
p-value = 0.097
Mean difference (Messi − Ronaldo) = 0.099 GC per game

Since p = 0.097 is above the 0.05 threshold, the difference is not statistically significant.

Conclusion

Across 19 matched seasons, Messi averaged about 0.1 more goal contributions per game than Ronaldo. However, a paired t-test found this difference was not statistically significant (t = 1.752, p = 0.097). This means that while Messi's numbers were slightly higher on average, the gap isn't large or consistent enough, relative to normal season-to-season variation, to confidently say one player was genuinely more effective than the other over this period. The honest takeaway is that the two players were statistically comparable in per-game scoring effectiveness, even though Messi held a slight numerical edge.

Tools Used
Python: pandas (data cleaning/reshaping), matplotlib/seaborn (visualization), scipy (paired t-test)
SQL: SQLite, including GROUP BY aggregation and window functions (RANK(), LAG())