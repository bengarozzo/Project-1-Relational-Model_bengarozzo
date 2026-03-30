# NBA Modeling Pipeline

## Purpose

The purpose of this notebook is to build a predictive pipeline for NBA game outcomes using the cleaned relational dataset. This includes loading cleaned tables, performing SQL-based validation, constructing a modeling dataset, training a model, and evaluating its performance through visualizations.

---

## Data Loading

The pipeline uses cleaned datasets generated from `create_project_data.ipynb`:

- `game_clean.csv`
- `line_score_clean.csv`
- `team_history_clean.csv`
- `other_stats_clean.csv`

These files are stored in the `data/cleaned/` directory and represent a relational dataset linked by `game_id` and `team_id`.

---

## DuckDB Setup and SQL Checks

The cleaned tables are registered in DuckDB to enable SQL-based querying.

### Row Count Validation

```sql
SELECT 'game' AS table_name, COUNT(*) FROM game
UNION ALL
SELECT 'line_score', COUNT(*) FROM line_score
UNION ALL
SELECT 'team_history', COUNT(*) FROM team_history
UNION ALL
SELECT 'other_stats', COUNT(*) FROM other_stats;
```

This confirms that all tables are loaded correctly.

---

### Season Summary

```sql
SELECT
    season_type,
    COUNT(*) AS game_count,
    AVG(pts_home) AS avg_home_points,
    AVG(pts_away) AS avg_away_points
FROM game
GROUP BY season_type
ORDER BY game_count DESC;
```

Key observations:

- Regular season games make up the majority of the dataset  
- Home teams score more on average than away teams  
- Minor inconsistencies in categorical values (e.g., "All Star" vs "All-Star") indicate real-world data quality issues  

---

### Join Validation

```sql
SELECT COUNT(*) AS matched_games
FROM game g
JOIN other_stats o
ON g.game_id = o.game_id;
```

This confirms that tables can be successfully joined using `game_id`, demonstrating the relational structure of the dataset.

---

## Feature Engineering

The modeling dataset is constructed using rolling averages of team performance metrics.

### Key Features

- Average points scored (`avg_pts`)
- Average rebounds (`avg_reb`)
- Average assists (`avg_ast`)
- Average turnovers (`avg_tov`)
- Shooting efficiency (`fg_pct`, `fg3_pct`, `ft_pct`)

### Methodology

- Each team is represented as one row per game  
- Rolling averages are calculated using the previous 10 games  
- A shift is applied to ensure only prior games are used  

This prevents data leakage by ensuring that no information from the current game is used to predict its outcome.

---

## Target Variable

The target variable is defined as:

```python
home_win = 1 if pts_home > pts_away else 0
```

This creates a binary classification problem where the goal is to predict whether the home team wins.

---

## Model

A logistic regression model is used with feature scaling:

- Model: Logistic Regression  
- Preprocessing: StandardScaler  
- Train/Test Split: 80/20  

This model was chosen for its interpretability and ability to estimate probabilities.

---

## Model Performance

The model achieved an accuracy of approximately **62.4%** on the test set.

### Confusion Matrix

- True Negatives: 939  
- False Positives: 2760  
- False Negatives: 743  
- True Positives: 4874  

The model performs better at predicting home wins than home losses, likely due to class imbalance and the presence of a home-court advantage in the data.

---

## Visualization and Analysis

### 1. Overall Accuracy

The model performs significantly better than random guessing (50%), indicating that historical team statistics contain predictive signal.

---

### 2. Confidence vs Accuracy

Prediction accuracy increases as model confidence increases.

This demonstrates that the model is well-calibrated: high-confidence predictions are more reliable than low-confidence ones.

---

### 3. Feature Importance

Field goal percentage is the most important predictor of winning, followed by rebounds and turnovers.

This aligns with established basketball analytics frameworks such as the "Four Factors," which emphasize efficiency and possession control.

---

### 4. Points Difference Distribution

Games where the home team has a higher prior scoring average are more likely to result in a win.

However, there is significant overlap between winning and losing distributions, indicating that scoring ability alone does not determine outcomes.

---

## Uncertainty

Although the model performs better than random guessing, it is far from perfect.

The overlap in feature distributions and the moderate accuracy (~62%) indicate that a substantial portion of game outcomes cannot be explained by historical statistics alone. This reflects inherent randomness in sports, as well as missing variables such as injuries, fatigue, and in-game dynamics.

---

## Summary

This pipeline demonstrates that NBA game outcomes can be partially predicted using historical performance data. The model captures meaningful relationships between team statistics and winning, while also highlighting the limitations of predictive modeling in a highly variable domain. The use of SQL, structured data, and rolling feature engineering provides a reproducible and interpretable workflow.