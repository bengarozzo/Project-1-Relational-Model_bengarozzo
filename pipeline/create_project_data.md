# Create Project Data

## Purpose

The purpose of this notebook is to construct a clean, usable dataset for modeling NBA game outcomes. This includes loading raw CSV files, validating their structure, performing SQL-based checks using DuckDB, and exporting cleaned tables for downstream analysis.

---

## Data Sources

The dataset is constructed from four CSV tables:

- `game.csv` — game-level statistics including scores and team performance  
- `line_score.csv` — quarter-by-quarter scoring breakdown  
- `team_history.csv` — historical team metadata  
- `other_stats.csv` — additional game-level statistics  

These tables are related through shared keys such as `game_id` and `team_id`, forming a relational dataset.

---

## Data Loading

All four CSV files are loaded into pandas DataFrames and validated to ensure required columns are present.

### Raw Table Shapes

- game: 65,698 rows  
- line_score: 58,053 rows  
- team_history: 52 rows  
- other_stats: 28,271 rows  

---

## DuckDB Setup and SQL Validation

The tables are registered in DuckDB to allow SQL-based querying and validation.

### Row Count Check

```sql
SELECT 'game' AS table_name, COUNT(*) AS row_count FROM game
UNION ALL
SELECT 'line_score', COUNT(*) FROM line_score
UNION ALL
SELECT 'team_history', COUNT(*) FROM team_history
UNION ALL
SELECT 'other_stats', COUNT(*) FROM other_stats;
```

All tables were successfully loaded with expected row counts.

---

### Season-Level Summary

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

- Regular season games dominate the dataset  
- Home teams score more on average than away teams  
- Inconsistent labels (e.g., "All Star" vs "All-Star") highlight real-world data quality issues  

---

### Join Validation

```sql
SELECT COUNT(*) AS matched_games
FROM game g
INNER JOIN line_score l
ON g.game_id = l.game_id;
```

Result:

- Most games successfully join across tables  
- Confirms the relational structure through the shared `game_id` key  

---

## Data Cleaning

The following cleaning steps were applied:

### Game Table
- Converted `game_date` to datetime  
- Removed rows with missing key fields (`game_id`, scores)  
- Removed duplicate game records  

### Line Score Table
- Converted date fields to datetime  
- Removed rows with missing or duplicate `game_id`  

### Team History Table
- Removed rows missing `team_id`  

### Other Stats Table
- Removed duplicate rows and rows with missing `game_id`  

---

## Cleaned Table Shapes

- game_clean: 65,642 rows  
- line_score_clean: 58,013 rows  
- team_history_clean: 52 rows  
- other_stats_clean: 28,261 rows  

---

## Data Export

Cleaned datasets are written to:

```
data/cleaned/
```

Files generated:

- `game_clean.csv`  
- `line_score_clean.csv`  
- `team_history_clean.csv`  
- `other_stats_clean.csv`  

These cleaned tables are used as the input for the modeling pipeline.

---

## Summary

This notebook constructs a relational dataset from multiple CSV sources, validates it using SQL queries, and applies cleaning steps to ensure consistency and usability. The resulting cleaned tables provide a reliable and reproducible foundation for modeling NBA game outcomes.