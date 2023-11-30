# 19-game rolling avg. for npxG and npxGA

## Introduction

This Python script collects and analyzes shooting data for a football team and its opponents from FBref for multiple seasons. The primary goals are to calculate rolling averages for expected goals (xG) and expected goals against (xGA), and then visualize the trends over time.

## Dependencies

- pandas
- time
- matplotlib.pyplot
- numpy

## Data Collection

The script iterates over specified seasons, constructs URLs for match logs on FBref, and retrieves shooting data for the football team and its opponents. The data is processed, including renaming columns and merging based on common identifiers.

## Data Analysis

### Rolling Averages

Rolling averages for xG and xGA are calculated using a window size of 19 matches. The values are rounded to two decimal places for clarity.

```python
df['xG_rolling'] = df['npxG_for'].rolling(window=19).mean()
df['xGA_rolling'] = df['npxG_against'].rolling(window=19).mean()
df = df.round(2)
```

## Data Visualization
The script utilizes matplotlib to create a plot with a black background. Rolling averages for xG and xGA are plotted using distinct colors. Season dividers are represented by grid lines with a specific color. Connecting lines between xG and xGA for each match provide additional insights.

```python
plt.plot(df.Match, df.xG_rolling, color=xg_clr)
plt.plot(df.Match, df.xGA_rolling, color=xga_clr)

for match, xg, xga in zip(df.Match, df.xG_rolling, df.xGA_rolling):
    plt.plot([match, match], [xg, xga], color=connector_clr, linestyle='--', linewidth=2, alpha=.5, zorder=1)
```

## Output
The resulting plot is saved as an image file in the 'Output' directory with a filename based on the team name and the metric ('-RollingXg.png').

```python
plt.savefig('Output/' + team + '-RollingXg.png', bbox_inches='tight')
```
