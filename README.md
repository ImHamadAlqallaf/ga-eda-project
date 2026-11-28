# YouTube Trending Videos: Views and Engagement

An exploratory data analysis (EDA) of YouTube trending videos, built with **Pandas**, **Matplotlib**, and **Seaborn** in a Jupyter Notebook. It explores **161,470 YouTube trending records** from the **US, Canada, France, and GB**, collected between **November 14, 2017 and June 14, 2018**, and looks at how **views** relate to **likes**, **comments**, and **publish dates**.

The results describe patterns in this dataset only. They do not prove what makes a video trend, and they do not represent all YouTube videos.

## Problem Statement

From November 14, 2017 to June 14, 2018, 161,470 YouTube trending records across four countries show view counts from 223 to about 424.5 million. The relationship between views and likes or comments is unclear, making it difficult to describe engagement among videos that reached the trending lists.

## Notebook Outline

1. **Part 1: Import libraries** — Load Pandas, Matplotlib, Seaborn, and Missingno.
2. **Part 2: View the data** — Load `data/youtube.csv`, inspect dimensions, column types, and initial summary statistics.
3. **Part 3: Check data quality** — Check for missing values, duplicate rows, broken `#NAME?` video IDs, and disabled comments/ratings or error flags.
4. **Part 4: Clean the data** — Build `newyoutube_df`, remove duplicates and `#NAME?` IDs, parse dates, and keep only the first trending record per video and country (`63,801` records).
5. **Part 5: Views, likes, and comments** — Filter out disabled ratings/comments (`62,155` records), inspect the views distribution, plot a correlation heatmap, calculate engagement rates per 100 views, and compare countries.
6. **Part 6: Publish dates** — Measure days from publishing to first trending day, track daily publishing volume and median views over time, and check whether busy publish days perform differently.
7. **Part 7: Summary and recommendations** — Summarize findings, share practical recommendations, and document dataset limitations.

## Dataset

- **File:** `data/youtube.csv` (about 83 MB)
- **Raw size:** 161,470 rows and 18 columns
- **Countries:** US, Canada, France, GB
- **Period:** November 14, 2017 to June 14, 2018
- **Main columns used:** `video_id`, `trending_date`, `publish_date`, `publish_country`, `views`, `likes`, `dislikes`, `comment_count`, `comments_disabled`, `ratings_disabled`, `video_error_or_removed`

Data quality and cleaning notes:

- **No missing values:** 0% missing across all 18 columns.
- **Broken IDs and duplicates:** 1,799 rows (1.11%) have `#NAME?` instead of a real `video_id`, and 219 rows (0.14%) are exact duplicates (ignoring `index`). Removing both drops 2,016 rows (1.25%, as 2 rows overlap), leaving 159,454 rows.
- **First trending day per video and country:** A video appears once for every day it trends, so keeping only its first trending record per `(video_id, publish_country)` pair leaves **63,801** records.
- **Disabled engagement flags:** Comments are disabled in 2,788 rows (1.73%) and ratings in 1,424 rows (0.88%). These rows are kept for views, but excluded from the likes/comments comparison (`engagement_df`, **62,155** rows) because disabled is not the same as zero engagement. Another 141 rows (0.09%) flagged as error/removed still have counts, so they are kept.
- **Date gaps:** Trending data covers 205 unique dates out of 213 days (missing January 10–11 and April 8–13, 2018).

> The original source of `data/youtube.csv` is not documented in this repository yet. See *Remaining Tasks*.

## Main Findings

1. **Views and likes** have a fairly strong positive relationship (**0.71** correlation). More views usually means more likes.
2. **Views and comments** have a weaker relationship (**0.53** correlation), while likes and comments are strongly linked (**0.82**). Views and dislikes have the weakest link (**0.41**).
3. **Typical engagement:** The median trending video got about **2.75 likes** and **0.38 comments** per 100 views. The middle half got between about **0.9 and 6.1 likes** per 100 views.
4. **Views are uneven:** On its first trending day, the median video had about **124,500** views (`124,496`), **2,700** likes (`2,727`), and **400** comments (`408`). About **91%** (`90.5%`) of videos had under 1 million views, while a few reached tens of millions (up to about 143 million).
5. **Timing:** About **77%** (`49,047` of `63,801`) first trended 1 day after publishing, and another `3,121` trended the same day. Median views doubled from around 100,000 to around 200,000 between March and June 2018.
6. **Busy publish days:** Most days had 250–350 new trending videos (busiest day: January 29, 2018 with 457). There is no clear link between how many videos were published on a day and their median views.
7. **Countries differ:** France has the most unique videos (**30,181**) but the lowest median views (about **45,000**), while Canada (**24,103**, ~221,000), the US (**6,281**, ~271,000), and GB (**3,236**, ~279,000) have higher medians.

## Recommendations

1. **Compare engagement using likes per 100 views,** not raw likes. About 1 to 6 likes per 100 views is normal for trending videos in this data.
2. **Check likes and comments in the first day or two** after publishing, because most videos reached the trending list the next day.
3. **Use medians, not averages,** when describing views, because a few huge videos pull the average up (`436,533` average vs `124,496` median).
4. **Look at each country separately,** because France has many more videos with lower views.
5. **Do not pick a "best day" to publish** from this data. No clear pattern was found between busy publish days and views.

## Limitations

- We removed **2,016 rows (1.25%)**: 1,799 with a `#NAME?` video ID and 219 exact duplicates.
- We only kept the first trending day per video and country, so we do not see how views grew after that.
- Trending data is missing for January 10 to 11 and April 8 to 13, 2018.
- The data covers only four countries from November 2017 to June 2018, so it does not represent all YouTube videos.
- All results are relationships. They do not show that likes, comments, or publish dates cause more views.

## Project Structure

```text
ga-eda-project/
├── README.md                          # Project documentation
├── youtube_trending_analysis.ipynb    # Main EDA notebook
├── youtube_trending_eda.pptx          # Project presentation slides
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

The data cleaning, analysis, visualisations, findings, recommendations, and presentation are complete.

### Remaining Tasks

- Confirm and document the original source of `data/youtube.csv`.

## To-Do

- [x] Create the PowerPoint presentation explaining the project, methodology, findings, visualisations, and recommendations.
- [ ] Create the project cover page.
