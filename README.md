UEFA 2022 Match Data Analysis (SQL & Datalab)

Project Overview:
An end-to-end data analysis project focusing on UEFA match statistics. The goal was to extract deep insights regarding team performances, home advantages, ball possession dominance, and player duel metrics using advanced SQL queries within a Datalab environment.

---

1. Best Home Advantage Team (Highest Home Wins):
SELECT 
    TEAM_NAME_HOME AS team_name,
    COUNT(*) AS home_wins
FROM SOCCER.TBL_UEFA_2022
WHERE TEAM_HOME_SCORE > TEAM_AWAY_SCORE
GROUP BY TEAM_NAME_HOME
ORDER BY home_wins DESC
LIMIT 1;

---

2. Majority Possession Dominance:
SELECT 
    team_name,
    COUNT(*) AS majority_possession_games
FROM (
    SELECT TEAM_NAME_HOME AS team_name 
    FROM SOCCER.TBL_UEFA_2022 
    WHERE POSSESSION_HOME > POSSESSION_AWAY
    
    UNION ALL
    
    SELECT TEAM_NAME_AWAY AS team_name 
    FROM SOCCER.TBL_UEFA_2022 
    WHERE POSSESSION_AWAY > POSSESSION_HOME
) AS combined_possession
GROUP BY team_name
ORDER BY majority_possession_games DESC
LIMIT 1;

---

3. Duels Won vs. Match Outcome:
SELECT 
    team_name,
    COUNT(*) AS duels_won_but_lost_match
FROM (
    SELECT TEAM_NAME_HOME AS team_name
    FROM SOCCER.TBL_UEFA_2022
    WHERE DUELS_WON_HOME > DUELS_WON_AWAY 
      AND TEAM_HOME_SCORE < TEAM_AWAY_SCORE

    UNION ALL

    SELECT TEAM_NAME_AWAY AS team_name
    FROM SOCCER.TBL_UEFA_2022
    WHERE DUELS_WON_AWAY > DUELS_WON_HOME 
      AND TEAM_AWAY_SCORE < TEAM_HOME_SCORE
) AS duel_losses
GROUP BY team_name
ORDER BY duels_won_but_lost_match DESC
LIMIT 1;

---
Tools & Technologies:
- Environment: Datalab / SQL Notebooks
- Database: Snowflake (SOCCER.TBL_UEFA_2022)
- Language: SQL (Aggregations, Subqueries, UNION ALL)
