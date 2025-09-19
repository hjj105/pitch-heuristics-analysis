==================== DATA ===========================================================

Three data files were used in the analysis:

1) fangraphs-leaderboards.csv
2) fangraphs-leaderboards_league.csv
3) fangraphs-leaderboards_team.csv

These three files share the same data with one exception. 

In fangraphs-leaderboards.csv, the Team variable is expressed as text. 

In fangraphs-leaderboards_league.csv, the Team variable is replaced with a League variable, 
coded as NL (National League) and AL (American League). 

This file is used for analyses that include league dummy variables. 

In fangraphs-leaderboards_team.csv, the Team variable is coded numerically from 1 to 30, 
with each number corresponding to a different team. 

This file is used for analyses that include team dummy variables.

Season refers to the statistics of batter i in year t (e.g., if Season = 2002, it indicates the 2002 performance of batter i).
Name refers to the name of batter i. 

Since duplicate names exist, the Name variable alone cannot uniquely identify players, 
which is problematic in analyses using lagged values 
(t-1 statistics as explanatory variables and t Zone Out% as the dependent variable). 
To address this, we used the PlayerId variable provided by FanGraphs to identify individual players.

The variables are defined as follows:

Age is the age of batter i in year t.
PA is the number of plate appearances.
HR is the number of home runs.
RBI is the number of runs batted in.
AVG is the batting average.
OBP is the on-base percentage.
SLG is the slugging percentage.
OPS is the on-base plus slugging.
SO is the number of strikeouts.
WAR is Wins Above Replacement.
Bat is the Batting Runs value.
Off is the Offensive Runs value.
wRC+ is the weighted Runs Created Plus.
WPA is the Win Probability Added.
Zone Out% is the percentage of pitches outside the strike zone faced by batter i in year t.

Other variables exist in the data files but were not used in the analysis, and are therefore not described here.


==================== STATA CODE ==================================================

Overview of Analysis Code Files

A total of 14 Stata .do files were created for the analysis. The names of the files are as follows:

Descriptive Statistics.do
ZoneOut_300.do
Standard stats_300.do
Advanced stats_300.do
All Year Regression.do
Previous Year Regression.do
Recent Year Regression.do
2024 Regression.do
All Year Regression_Except CV, Dummies.do
All Year Regression_Except CV.do
Comparison_HR_Bat.do
Comparison_HR_wRC.do
HR_Bat_Graph_300PA.do
HR_wRC_Graph_300PA.do

Each .do file begins by importing the dataset using the following command:

import delimited "file_path\fangraphs-leaderboards.csv", case(preserve)


Note: Users should modify the file_path according to their local directory.

Be sure to check the exact file name: whether it is 
fangraphs-leaderboards.csv, 
fangraphs-leaderboards_team.csv, 
or fangraphs-leaderboards_league.csv.

Descriptive Statistics.do

This script generates the descriptive statistics required for Table 1.
Players with zero plate appearances are excluded.
The file sequentially calculates summary statistics for the dependent and independent variables based on the following filters:

All players
Players with กร 100 PA (2020 values are adjusted by *(37/100))
Players with กร 200 PA
Players with กร 300 PA
Players with กร 400 PA
Players with กร 500 PA

The final section computes pairwise correlations among the independent variables under each PA threshold.
ZoneOut_300.do, Standard stats_300.do, Advanced stats_300.do

These scripts generate the year-by-year trend lines for Figure 1, based on players with 300+ PA.
ZoneOut_300.do: Plots the dependent variable (Zone Out %)
Standard stats_300.do: Plots traditional statistics
Advanced stats_300.do: Plots advanced statistics

At the end of each script, the following command adjusts graph formatting to ensure consistent figure dimensions:
graphregion(margin(4.07 17.8 0 0)) ///
legend(on order(1 "Zone Out%") position(6) row(1))

Regression Files
The following files conduct regression analyses:

All Year Regression.do
Previous Year Regression.do
Recent Year Regression.do
2024 Regression.do
All Year Regression_Except CV, Dummies.do
All Year Regression_Except CV.do

All files follow the same procedure:

Create an Ageฉ๗ variable
Standardize all independent variables, dependent variables, and control variables
Perform regressions across six PA thresholds: all players, 100+, 200+, 300+, 400+, and 500+ PA

Details:
All Year Regression.do: For Table 2, covering 2002?2024
Previous Year Regression.do: For Table 3, using lagged (t-1) independent variables
Recent Year Regression.do: For Table A1, analyzing only 2019?2024
2024 Regression.do: For Table A2, analyzing only 2024
All Year Regression_Except CV, Dummies.do: For Table 2a (from Table 2 revision file), excludes control and dummy variables
All Year Regression_Except CV.do: For Table 2b (from Table 2 revision file), excludes control variables only

Note:
Table A2: The effects of control variables on Zone Out% can be constructed using regression outputs from All Year Regression.do.

Comparison Files

Comparison_HR_Bat.do: Conducts z-tests comparing the coefficients of HR vs. Bat
Comparison_HR_wRC.do: Conducts z-tests comparing HR vs. wRC+

These results populate Table 4.
Graphing Files for Figure 2

HR_Bat_Graph_300PA.do: Creates Figure 2a (HR vs. Bat, 300 PA threshold)
HR_wRC_Graph_300PA.do: Creates Figure 2b (HR vs. wRC+, 300 PA threshold)

