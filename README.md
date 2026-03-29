# DS 4320 Project 1: Predicting NBA Game Outcomes with Team Performance Data

## Executive Summary
This repository contains my DS 4320 Project 1. The goal of this project is to build a relational secondary dataset and use it to study whether team performance statistics can predict NBA game outcomes. The repository includes the project writeup, background readings, dataset documentation, a press release, and a proof-of-concept pipeline built with Python, SQL, and DuckDB.

## Name
Ben Garozzo

## NetID
huk5pd

## DOI
[Add DOI here]

## Press Release
[Press Release](./press_release.md)

## Data
[Data Folder](./data)

## Pipeline
[Pipeline Folder](./pipeline)

## License
[MIT License](./LICENSE)

## Problem Definition

### Initial General Problem and Refined Specific Problem
**Initial General Problem:** Predicting sports game outcomes.

**Refined Specific Problem:** Can team offensive and defensive statistics be used to predict which team will win an NBA game?

### Rationale for Refinement
The general problem of predicting sports game outcomes is too broad because it could apply to many sports, leagues, and prediction targets. I refined the problem to NBA games and to predicting the winner of each game because the available data are structured, large enough for analysis, and naturally relational. This makes the project specific, measurable, and manageable.

### Motivation for the Project
NBA teams, analysts, and fans increasingly use data to understand performance and decision-making. Team statistics such as scoring, shooting efficiency, rebounds, and turnovers can reveal patterns related to winning. This project is motivated by the idea that historical team performance data may help explain and predict game outcomes in a useful and interpretable way.

### Headline of Press Release and Link
**Headline:** Can Basketball Data Predict Who Wins the Game?

[Read the Press Release](./press_release.md)

## Domain Exposition

### Terminology
| Term | Definition |
|---|---|
| Offensive Rating | Estimated points scored per 100 possessions. |
| Defensive Rating | Estimated points allowed per 100 possessions. |
| Possession | A period when one team controls the ball until possession changes. |
| Field Goal Percentage | The share of shot attempts that are made. |
| Rebounds | The number of times a team gains the ball after a missed shot. |
| Assists | Passes that directly lead to made baskets. |
| Turnovers | Times a team loses possession. |
| Sports Analytics | The use of data and statistical methods to study sports performance. |

### Domain Explanation
This project lives in the domain of sports analytics. In basketball, analysts use game and team statistics to understand why teams win or lose and to evaluate performance over time. NBA data are useful for this because they are structured at the game level and include both outcome variables and performance measures. This project uses those statistics to study whether team-level performance can be used to predict game outcomes.

### Background Reading
See the [background_reading](./background_reading) folder.

### Reading Summary Table

| Title | Brief Description | Link |
|---|---|---|
| Hybrid Basketball Game Outcome Prediction Model | Applies machine learning techniques to NBA data to predict game outcomes. | [paper](background_reading/paper1_hybrid_model.pdf) |
| GCN + Random Forest Basketball Prediction | Combines graph neural networks and random forests to improve prediction accuracy. | [paper](background_reading/paper2_gcn_rf.pdf) |
| Predicting the Winning Team in Basketball | Explores statistical patterns in basketball data to predict winners. | [paper](background_reading/paper3_novel_approach.pdf) |
| XGBoost + SHAP NBA Prediction | Uses XGBoost and SHAP to explain which stats drive winning. | [paper](background_reading/paper4_xgboost_shap.pdf) |
| Basketball Reference Four Factors | Explains key basketball metrics that influence winning games. | [paper](background_reading/paper5_four_factors.pdf) |

## Data Creation

### Provenance
The dataset was created using the public Kaggle basketball dataset by Wyatt Walsh. The data were downloaded in Google Colab using the kagglehub package. From the full dataset, I selected game.csv, line_score.csv, team_history.csv, and other_stats.csv because they contain the game outcomes, team identifiers, and performance statistics needed for this project.

### Code Table
| File | Brief Description | Link |
|---|---|---|
| create_project_data.ipynb | Downloads and loads the source data. | ./pipeline/create_project_data.ipynb |
| create_project_data.md | Markdown export of the data creation notebook. | ./pipeline/create_project_data.md |
| nba_pipeline.ipynb | Loads data into DuckDB, prepares features, and builds the model. | ./pipeline/nba_pipeline.ipynb |
| nba_pipeline.md | Markdown export of the pipeline notebook. | ./pipeline/nba_pipeline.md |

### Bias Identification
Bias may enter this dataset because it only includes recorded game data and may omit important contextual factors such as injuries, travel, rest, coaching strategy, and roster changes. It may also reflect differences across seasons in style of play or recording practices.

### Bias Mitigation
Bias can be reduced by using multiple seasons, including a range of team performance variables, and being explicit that the model predicts from recorded game statistics rather than the full basketball context. Results should be interpreted as conditional on the available data.

### Rationale for Critical Decisions
I selected a small set of core tables to keep the relational structure manageable while still supporting meaningful prediction. I also chose team-level rather than player-level modeling because it matches the project goal and reduces complexity. The main uncertainty comes from joining tables correctly on shared IDs and deciding which statistics are most informative without introducing leakage.

## Metadata

### Schema
[Add ER diagram or description here]

### Data Table
| Table Name | Description | Link |
|---|---|---|
| game | Main game-level table with teams, date, outcome, and statistics. | ./data/game.csv |
| line_score | Quarter and overtime scoring by game. | ./data/line_score.csv |
| team_history | Team identity and history information. | ./data/team_history.csv |
| other_stats | Additional game-level statistics. | ./data/other_stats.csv |

### Data Dictionary
| Feature Name | Data Type | Description | Example |
|---|---|---|---|
| game_id | integer | Unique identifier for each game | 0022100001 |
| game_date | string | Date of game | 2021-10-19 |
| team_id_home | integer | Home team ID | 1610612737 |
| team_id_away | integer | Away team ID | 1610612738 |
| pts_home | integer | Home team points | 110 |
| pts_away | integer | Away team points | 105 |

### Numerical Uncertainty
| Feature | Quantified Uncertainty |
|---|---|
| pts_home / pts_away | Add mean, standard deviation, minimum, and maximum across games. |
| fg_pct_home | Add mean and standard deviation across games. |
| reb_home | Add mean, standard deviation, minimum, and maximum across games. |

## Pipeline Check
The pipeline loads the selected CSV files into DuckDB, joins the tables on shared keys, creates features, fits a prediction model, and generates an output chart. The notebook and markdown export are linked in the pipeline folder.
