# Vietnam National Football Team Performance Analysis (2010-2019)

## Introduction

In this project, we will perform analysis on the performance data of the Vietnam National Football Team from 2010 to 2019, considering only official matches within the framework of FIFA.

## Data

The data for this project is sourced from the [Vietnam National Football Team Results Wikipedia page](https://en.wikipedia.org/wiki/Vietnam_national_football_team_results).

The dataset includes the following columns:
- `date`: The date of the match.
- `opponent`: The opponent team.
- `score`: The match score (only counted within 90 minutes of official competition). The number of goals scored by the Vietnamese team is always written first.
- `venue`: The venue of the match.
- `venue_type`: The position of the Vietnam Team:
  - `H`: Home match.
  - `A`: Away match.
  - `N`: Neutral ground.
- `competition`: The tournament in which the Vietnamese Team participated.

## Exercise

### Question 1: Read Data into DataFrame,

```python
import pandas as pd
data = pd.read_csv('Data.txt')
data
```
### Question 2: Check the data types of the columns and make the necessary conversions.
```python
data = data.assign(
    date = pd.to_datetime(data.date)
)
data.dtypes
```
### Question 3: How many matches did the Vietnamese team play in the period 2010 - 2019, including how many matches were played at home, away and neutral stadiums.
```python
print('Total number of match:',data.shape[0])
venue = data.venue_type.unique()
for name_venue in venue:
  print('''Position play Việt Nam's team:''' , name_venue , ':' , 'Number of match:',data[data['venue_type'] == name_venue].shape[0])
```
### Question 4: In home matches, how many different locations does the Vietnamese Team play at? Print out those locations.
```python
place_H = data[data['venue_type'] == 'H']
print('Số địa điểm sân nhà :', place_H.venue.nunique())
place_H.venue.unique()
```
### Question 5: On average, how many matches does the Vietnamese team play each year?
```python
year_play = data.date.dt.year.nunique()
print('Average number of match each year:', data.shape[0] / year_play)
```
### Question 6: Print out the number of matches the Vietnamese team played in 2015.
```python
print('Number of match took place in 2015:',data[data['date'].dt.year == 2015].shape[0])
```
### Question 7: From the `score` column, create two new columns:
- `goal`: number of goals scored by the Vietnamese team.
- `conceded_goal`: number of goals conceded by the Vietnamese Team.

```python
score = data['score']
goal = []
conceded_goal = []
for macth in score:
  goal.append(macth[:1])
  conceded_goal.append(macth[2:])
data = data.assign(
    goal = goal,
    conceded_goal = conceded_goal
)
data
```
### Question 8: Print out the matches in which the Vietnamese team scored the most goals?
```python
max_goal = data.goal.max()
data[data['goal'] == max_goal]
```

### Question 9: Calculate the average number of goals scored by the Vietnamese team. (Number of goals # Number of wins)
```python
data = data.assign(
    goal = pd.to_numeric(goal),
    conceded_goal = pd.to_numeric(conceded_goal)
)
mean_goal = data.goal.mean()
print('Số trận trung bình:' ,mean_goal)
```
### Question 10: Calculate the winning rate of the Vietnamese Team.
```python
win = 0
for macth in range(data.shape[0]):
  if data.iloc[macth].goal > data.iloc[macth].conceded_goal:
    win = win + 1
print('Win rate:' ,(100/data.shape[0])*win)
```
### Question 11: Select friendly matches during the above period. Know that friendly matches are recorded as `'Friendly'` in `competition`.
```python
data_friendly = data[data.competition.str.find('Friendly') != -1]
data_friendly
```

### Question 12: Calculate the win rate of the Vietnamese Team in friendly matches.
```python
win_friendly = 0
for macth_friendly in range(data_friendly.shape[0]):
  if data_friendly.iloc[macth_friendly].goal > data_friendly.iloc[macth_friendly].conceded_goal:
    win_friendly = win_friendly + 1
print('Win rate:' ,(100/data_friendly.shape[0])*win_friendly)
```

### Question 13: Average number of goals conceded in **non** friendly matches.
```python
data_not_friend = data[data.competition.str.find('Friendly') == -1]
lost = 0
for macth_lost in range(data_not_friend.shape[0]):
  if data_not_friend.iloc[macth_lost].goal < data_not_friend.iloc[macth_lost].conceded_goal:
    lost = lost + 1
print('Lose rate:' ,(100/data_not_friend.shape[0])*lost)
```
## Contact Information

For further information, collaboration, or inquiries, feel free to contact [Phil Dinh](mailto:dinhthanhtrung2011@gmail.com).
