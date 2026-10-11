# NYC Public Schools: SAT Performance Analysis

A pandas exploratory analysis of SAT results across 375 New York City public schools, completed as a guided practice project on DataCamp's DataLab while building my data skills toward data engineering and data science.

## Overview

SAT scores (reading, math and writing, each up to 800 points, so 2400 in total) are one of the inputs that policymakers, researchers and families use to compare schools. This project answers three questions about NYC school-level results:

1. Which schools have a math average above 80% of the maximum score?
2. Which 10 schools have the highest total SAT score?
3. Which borough shows the widest spread in school performance?

## Dataset

`schools.csv` has one row per school (375 rows) with the school name, borough, building code, and average reading, math and writing scores. The dataset and task framing come from DataCamp's guided project.

## Approach

- Filtered schools whose average math score is above 80% of 800 (above 640), grouped by school name and ranked them by mean math score
- Created a `total_SAT` column by summing the average reading, math and writing scores
- Sorted by `total_SAT` to get the top 10 schools
- Grouped by borough to compute the number of unique schools, mean total SAT and standard deviation (rounded to 2 decimals), then sorted by standard deviation to find the most variable borough

A simplified version of the core logic:

```python
schools["total_SAT"] = (
    schools["average_reading"] + schools["average_math"] + schools["average_writing"]
)

borough_stats = schools.groupby("borough").agg(
    num_schools=("school_name", "nunique"),
    average_SAT=("total_SAT", "mean"),
    std_SAT=("total_SAT", "std"),
).round(2)
```

## Key findings

**1. Ten schools clear the 80% math threshold.** Out of 375 schools, these ten have an average math score above 640, listed here from highest to lowest math average:

- Stuyvesant High School
- Bronx High School of Science
- Staten Island Technical High School
- Queens High School for the Sciences at York College
- High School for Mathematics, Science, and Engineering at City College
- Brooklyn Technical High School
- Townsend Harris High School
- High School of American Studies at Lehman College
- New Explorations into Science, Technology and Math High School
- Eleanor Roosevelt High School

**2. Top 10 schools by total SAT score.** Stuyvesant High School leads with 2144 out of 2400, more than 100 points ahead of the next two schools.

| School | Total SAT |
|---|---|
| Stuyvesant High School | 2144 |
| Bronx High School of Science | 2041 |
| Staten Island Technical High School | 2041 |
| High School of American Studies at Lehman College | 2013 |
| Townsend Harris High School | 1981 |
| Queens High School for the Sciences at York College | 1947 |
| Bard High School Early College | 1914 |
| Brooklyn Technical High School | 1896 |
| Eleanor Roosevelt High School | 1889 |
| High School for Mathematics, Science, and Engineering at City College | 1889 |

**3. Strong math and strong overall results mostly go together, with two exceptions.** Nine of the ten schools above the math threshold also appear in the top 10 by total score. New Explorations into Science, Technology and Math High School has a math average of 657 but is not in the overall top 10. Bard High School Early College is in the top 10 (1914) even though its math average of 634 is just under the cutoff, because its three sections are balanced (about 634 math, 641 reading, 639 writing).

**4. Manhattan has the widest spread in school performance.** Of all boroughs, Manhattan has the highest standard deviation of total SAT scores (230.29), across 89 schools with a mean total of 1340.13. In other words, Manhattan's school results differ from one another more than in any other borough. This analysis does not explain why.

## Limitations

- The data holds school-level averages only, with no student counts. Every school counts equally in the borough mean and standard deviation, so the spread shown is variation between schools, not between students.
- The analysis is descriptive. The dataset has no demographic, funding or admissions information, so it cannot explain why scores differ.
- The 80% math cutoff is a fixed threshold set by the task. Changing it changes which schools qualify.

## Tools

Python, pandas

## Repository contents

```
nyc-schools-sat-analysis/
├── README.md
├── nyc_schools_sat_analysis.ipynb
└── schools.csv
```
