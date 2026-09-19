# ECE2112-PA4 | Dirk Lendl E. Villaluna | 2ECE-A

# Overview
This repository contains the completed Programming Assignment 4 for the course ECE2112 - Advanced Programming and Algorithms for the Academic Year 2026-27. The  topic coverage of the said programming assignment is Data Wrangling and Data Visualization using Pandas and Matplotlib.

## SETUP
Load the provided dataset board2.csv and derive the row-wise Average score for each examinee across their exam subjects (Math, GEAS, and Electronics).

Requirements to follow:

1. Do not hardcode values; load and derive directly from the dataset.
2. Compute the Average column dynamically across the three exam subject columns.

Methods & Functions Used:
- import pandas as pd: Imports the Pandas Library for data manipulation.
- pd.read_csv('board2.csv'): Loads the CSV file into a DataFrame df.
- df[['Math', 'GEAS', 'Electronics']].mean(axis=1): Vectorized row-wise calculation to compute the average of the three test scores.

Implementation of code:

```
import pandas as pd

df = pd.read_csv('board2.csv')
df['Average'] = df[['Math', 'GEAS', 'Electronics']].mean(axis=1)

```

## A. VISAYAS COMMUNICATION EXAMINEES PROBLEM

Filter the dataset for students whose hometown is in Visayas and whose track is Communication. Store the resulting DataFrame in VisComm, retaining only the specified columns: Name, Gender, Math, Electronics, and Average.

Requirements to follow:
1. Make all filter conditions explicit using Boolean indexing.
2. Filter rows before selecting columns, keeping the original DataFrame df unchanged.
3. Display the resulting DataFrame and the total number of rows.

Methods & Functions Used:
- (df['Hometown'] == 'Visayas') & (df['Track'] == 'Communication'): Combined Boolean indexing condition to filter matching rows.
- [cols_a]: Bracket indexing to project and order the specified subset of columns.
- display(VisComm): Renders the filtered DataFrame as a structured table.
- len(VisComm): Determines the total number of selected rows.

Implementation of code:


```
# Specifies the columns to be retained
cols_a = ['Name', 'Gender', 'Math', 'Electronics', 'Average']

# Filters the dataset for Communication track students from Visayas
VisComm = df[(df['Hometown'] == 'Visayas') & (df['Track'] == 'Communication')][cols_a]

# Displays the resulting table and row count
display(VisComm)
print("Number of rows:", len(VisComm))
```

## B. VISAYAS FEMALE EXAMINEES PROBLEM

Filter the dataset for female students whose hometown is in Visayas and store the result in VisFemale, retaining Name, Track, GEAS, Electronics, and Average. Additionally, display the subset of female students with an Average score of at least 60, without overwriting or mutating VisFemale.

Requirements to follow:
1. Maintain the original VisFemale DataFrame intact in memory.
2. Filter the passing subset (Average >= 60) into a separate variable or output.
3. Display both the full VisFemale table and the filtered subset of passing.

Methods & Functions Used:
- (df['Hometown'] == 'Visayas') & (df['Gender'] == 'Female'): Compound condition filtering examinees by region and gender.
- [cols_b]: Selects the required columns in the specified order.
- VisFemale['Average'] >= 60: Boolean mask filtering rows where the examinee's average is at least 60.
- display(): Renders both tabular outputs.

Implementation of code:

```
# Specifies the columns to be retained
cols_b = ['Name', 'Track', 'GEAS', 'Electronics', 'Average']

# Filters the dataset for female students from Visayas
VisFemale = df[(df['Hometown'] == 'Visayas') & (df['Gender'] == 'Female')][cols_b]

# Displays the "VisFemale" table
print("--- VisFemale (All) ---")
display(VisFemale)

# Filters and displays female students whose Average is at least 60
vis_female_passed = VisFemale[VisFemale['Average'] >= 60]

print("\n--- VisFemale (Average >= 60) ---")
display(vis_female_passed)
```

## C. CATEGORY-AVERAGE VISUALIZATION & INTERPRETATION PROBLEM

Compute the mean of Average for each category under Track, Gender, and Hometown. Display the summary tables, visualize them side-by-side using a 3-subplot bar chart figure sharing a common y-axis, and state the categories with the highest sample mean average without asserting causal claims.

Requirements to follow:
1. Compute and display summary tables for all three categorical features.
2. Render one Matplotlib figure with three subplots sharing the same y-axis scale (sharey=True).
3. Label all axes, set individual subplot titles, and ensure tick readability.
4. Programmatically extract the top-performing categories and print concise summary sentences.

Methods & Functions Used:
- df.groupby('...') [['Average']].mean(): Groups data by categorical feature and computes the mean overall score.
- plt.subplots(nrows=1, ncols=3, sharey=True): Creates a 1x3 subplot grid with synchronized y-axes.
- ax.bar(): Renders categorical bar charts with distinct color styles.
- .sort_values(by='Average', ascending=False).index[0]: Dynamically finds the category with the highest sample mean.
- plt.tight_layout() & plt.show(): Formats subplot spacing and renders the complete figure.

Implementation of code:

```
import matplotlib.pyplot as plt

# Computes the mean of "Average" grouped by Track, Gender, and Hometown
mean_track = df.groupby('Track')[['Average']].mean()
mean_gender = df.groupby('Gender')[['Average']].mean()
mean_hometown = df.groupby('Hometown')[['Average']].mean()

# Displays the summary tables rounded to two decimal places
print("--- Mean Average by Track ---")
display(mean_track.round(2))

print("\n--- Mean Average by Gender ---")
display(mean_gender.round(2))

print("\n--- Mean Average by Hometown ---")
display(mean_hometown.round(2))

# Creates a single figure with three subplots sharing the same y-axis scale
fig, axes = plt.subplots(nrows=1, ncols=3, figsize=(15, 5), sharey=True)

# Generates the bar chart for Track
axes[0].bar(mean_track.index, mean_track['Average'], color='skyblue', edgecolor='black')
axes[0].set_title('Mean Average by Track')
axes[0].set_xlabel('Track')
axes[0].set_ylabel('Mean Average')
axes[0].tick_params(axis='x', rotation=15)

# Generates the bar chart for Gender
axes[1].bar(mean_gender.index, mean_gender['Average'], color='salmon', edgecolor='black')
axes[1].set_title('Mean Average by Gender')
axes[1].set_xlabel('Gender')

# Generates the bar chart for Hometown
axes[2].bar(mean_hometown.index, mean_hometown['Average'], color='lightgreen', edgecolor='black')
axes[2].set_title('Mean Average by Hometown')
axes[2].set_xlabel('Hometown')

# Adjusts layout spacing and renders the figure
plt.tight_layout()
plt.show()

# Determines the categories with the highest sample mean Average score
best_track = mean_track.sort_values(by='Average', ascending=False).index[0]
best_gender = mean_gender.sort_values(by='Average', ascending=False).index[0]
best_hometown = mean_hometown.sort_values(by='Average', ascending=False).index[0]

# Prints the interpretation statements describing the highest categories
print(f"For Track, {best_track} had the highest sample mean Average score.")
print(f"For Gender, {best_gender} had the highest sample mean Average score.")
print(f"For Hometown, {best_hometown} had the highest sample mean Average score.")
```

# THANK YOU SO MUCH FOR READING!!! :D
## To view the main Python program, open the .ipynb file included in this repository. Thank you!
