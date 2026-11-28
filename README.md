# YouTube Trending Videos: Views and Engagement

An exploratory data analysis (EDA) of YouTube trending videos, built with **Pandas**, **Matplotlib**, and **Seaborn** in a Jupyter Notebook. It looks at how views relate to likes and comments for videos that reached the trending lists in four countries.

## Project Overview

The dataset has 161,470 trending records from the US, Canada, France, and GB, recorded between November 14, 2017 and June 14, 2018. The notebook checks the data quality, cleans it, and compares views with likes, comments, and publish dates using simple statistics and charts.

The results describe patterns in this dataset. They do not prove what causes a video to trend, and they do not represent every video on YouTube.

## Problem Statement

From November 14, 2017 to June 14, 2018, 161,470 YouTube trending records across four countries show view counts from 223 to about 424.5 million. The relationship between views and likes or comments is unclear, making it difficult to describe engagement among videos that reached the trending lists.

## Objectives

1. Describe the dataset and check its data quality.
2. Prepare a consistent analysis table with valid video-country observations.
3. Measure how views relate to likes and comments.
4. Summarize the findings and limitations, then suggest cautious follow-up questions.

### How the objectives answer the problem statement

| Problem | Objective that addresses it |
| --- | --- |
| Invalid IDs and repeated daily records can distort the numbers | 1 and 2: find and remove `#NAME?` IDs and duplicates, keep one record per video and country |
| The relationship between views and likes or comments is unclear | 3: correlation heatmap, views distribution, and likes/comments per 100 views |
| Engagement among trending videos is hard to describe | 4: plain-language findings, limitations, and recommendations |

### SMART criteria

- **Specific:** Views and their relationship with likes and comments in the four-country dataset.
- **Measurable:** The direction and strength of each relationship using correlation and engagement rates.
- **Achievable:** The dataset contains all three measures. Repeated records, invalid IDs, and disabled comments or ratings are handled.
- **Relevant:** The results describe engagement among videos on these trending lists.
- **Time-bound:** Data from November 14, 2017 to June 14, 2018. Analysis completed by the course submission date.

### Scope

- **In scope:** `views` as the dependent variable, compared with `likes`, `comment_count`, and publish timing. One first-recorded observation per valid video-country pair. Results described as relationships, not causes.
- **Out of scope:** What causes a video to trend, results for all YouTube videos, and modelling category, tags, or publishing time as predictors.

## Dataset

- **File:** `data/youtube.csv` (about 83 MB)
- **Size:** 161,470 rows and 18 columns
- **Countries:** US, Canada, France, GB
- **Period:** November 14, 2017 to June 14, 2018
- **Main columns used:** `video_id`, `trending_date`, `publish_date`, `publish_country`, `views`, `likes`, `dislikes`, `comment_count`, `comments_disabled`, `ratings_disabled`

Data quality notes:

- No null or blank cells.
- 1,799 rows have `#NAME?` instead of a video ID. These are removed.
- 219 rows are exact duplicates (ignoring the `index` column). These are removed.
- A video appears once for every day it trends, so only its first trending record per country is kept. This leaves 63,801 records.
- Comments are disabled in 2,788 rows and ratings in 1,424 rows. These are left out of the engagement comparison, because disabled is not the same as zero.
- Trending data is missing for January 10 to 11 and April 8 to 13, 2018 (205 trending dates out of 213 days).

> The original source of the CSV is not documented in this repository yet. See *Remaining Tasks*.

## Main Findings

1. **Views and likes** have a fairly strong positive relationship (`0.71` correlation). Videos with more views usually have more likes.
2. **Views and comments** have a weaker relationship (`0.53` correlation). Likes and comments are strongly related to each other (`0.82`).
3. **Typical engagement** is about `2.75` likes and `0.38` comments per 100 views (median).
4. **Views are uneven:** the median video had about 124,500 views on its first trending day, and about 91% of videos had under 1 million views, while a few reached tens of millions.
5. **Timing:** about 77% of videos (49,047 of 63,801) first appeared on the trending list 1 day after publishing. Median views rose from around 100,000 to around 200,000 between March and June 2018.
6. **Busy publish days:** there is no clear pattern showing that publishing on a busier or quieter day leads to higher views.
7. **Countries differ:** France has the most unique videos (30,181) but the lowest median views (about 45,000), while Canada, US, and GB range from about 220,000 to 280,000.

## Recommendations

- Compare engagement using **likes per 100 views** rather than raw likes (about 1 to 6 likes per 100 views is typical for trending videos in this dataset).
- Check likes and comments in the **first day or two** after publishing, since most videos trended the next day.
- Report **medians instead of averages**, because a few huge videos pull the average up.
- Look at **each country separately**, because France has many more videos with lower views.
- Do not recommend a "best day" to publish based on this dataset, as no clear link was found between daily publishing volume and views.
- Remember these are observed relationships, not proof that likes, comments, or dates cause higher views.

## Project Structure

```text
ga-eda-project/
├── README.md                          # Project documentation
├── youtube_trending_analysis.ipynb    # Main EDA notebook
├── charts/                            # Saved PNG charts for the presentation
└── data/
    └── youtube.csv                    # YouTube trending dataset
```

## How to Run

1. **Install dependencies:**
   ```bash
   pip install pandas matplotlib seaborn missingno jupyter
   ```

2. **Open the notebook:**
   ```bash
   jupyter notebook youtube_trending_analysis.ipynb
   ```

3. Run all cells from top to bottom.

## Project Status

The data cleaning, analysis, visualisations, findings, and recommendations in the notebook are complete.

### Remaining Tasks

- Confirm and document the original source of `data/youtube.csv`.

## To-Do

- [ ] Create the PowerPoint presentation explaining the project, methodology, findings, visualisations, and recommendations.
- [ ] Create the project cover page.
