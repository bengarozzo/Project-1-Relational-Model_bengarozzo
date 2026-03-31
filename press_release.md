# Can Recent Performance Predict Who Wins an NBA Game?

## Hook

Every NBA game comes with endless stats, but one question matters most: who is going to win? This project takes complex basketball data and turns it into a simple, practical insight by using recent team performance to estimate the probability that a team will win its next game.

## Problem Statement

NBA teams generate large amounts of data every game, including points, rebounds, assists, turnovers, and shooting percentages. However, it is not always clear how these statistics translate into actual wins. Looking at a single game can be misleading because outcomes are influenced by randomness, short-term streaks, and game-specific factors.

The key problem is determining whether a team’s recent performance provides meaningful information about future outcomes. Specifically, this project focuses on predicting whether the home team will win using only information that would be available before the game begins.

## Solution Description

To address this problem, I built a structured dataset combining multiple NBA data tables and created a predictive model based on recent team performance. Instead of using raw game data, the model uses rolling averages from each team’s previous games, including scoring, rebounds, assists, turnovers, and shooting efficiency.

A logistic regression model was then trained to estimate the probability that the home team wins. The model achieved approximately **62.4% accuracy**, showing that recent performance does provide useful predictive signal. The results also highlight that certain factors, especially shooting efficiency and turnovers, play a larger role in determining outcomes.

This approach provides a simple and interpretable way for analysts, coaches, or fans to understand how recent team trends translate into win probability.

## Chart

The chart below shows how a team’s recent scoring advantage affects both predicted and actual win outcomes.

- Each dot represents a game  
- The x-axis shows how much better the home team has been scoring compared to the away team  
- The y-axis shows the probability that the home team wins  
- The solid line shows the model’s predictions  
- The dashed line shows what actually happens in real games  

As the home team’s scoring advantage increases, both the predicted probability and actual win rate increase. However, the spread of points shows that outcomes are not certain, even when one team appears stronger.

<img width="924" height="575" alt="Screenshot 2026-03-31 at 12 50 47 PM" src="https://github.com/user-attachments/assets/1209a5cc-4d75-4b45-aceb-dbfb7f9a11ef" />
