# YouTube trending videos: views and engagement

## Project overview

This project explores how views relate to likes and comments among videos recorded on YouTube's trending lists. The dataset covers the US, Canada, France, and GB from November 14, 2017 to June 14, 2018. It contains 161,470 records.

The analysis describes associations in this dataset. It does not test what causes a video to trend, and it does not represent every video uploaded to YouTube.

## Problem statement

From November 14, 2017 to June 14, 2018, 161,470 YouTube trending records across four countries show view counts from 223 to about 424.5 million. The relationship between views and likes or comments is unclear, making it difficult to describe engagement among videos that reached the trending lists.

## SMART criteria

- **Specific:** Focus on views and their relationship with likes and comments in the four-country dataset.
- **Measurable:** Report the direction and strength of the associations between views and likes, and between views and comments.
- **Achievable:** The dataset contains these measures. The analysis will check repeated records, invalid video IDs, and disabled comments or ratings.
- **Relevant:** The results will describe engagement patterns among the videos in these trending lists.
- **Time-bound:** The source data spans November 14, 2017 to June 14, 2018. Complete the analysis by the course project submission date.

## Scope

### In scope

- Use `views` as the dependent variable.
- Compare views with `likes` and `comment_count`.
- Use one first-recorded observation per valid video-country pair to limit repeated daily records.
- Check missing values, date formats, duplicate records, and the `comments_disabled` and `ratings_disabled` flags.
- Describe results as associations, not causes.

### Out of scope

- Explaining or proving what causes a video to trend.
- Generalizing the results to all YouTube videos or current YouTube activity.
- Modeling category, tags, publishing time, or other factors as additional predictors.
- Making recommendations that claim to cause higher views.

## Variables

- **Dependent variable:** `views`
- **Comparison variables:** `likes` and `comment_count`
- **Data-handling flags:** `ratings_disabled` and `comments_disabled`
- **Observation unit:** One first-recorded entry for each valid video-country pair.

Disabled ratings or comments are not the same as zero engagement. The corresponding records will be handled separately in each comparison.

## Data notes

- The CSV has no reported null or blank cells, but 1,799 `video_id` values contain `#NAME?`. Exclude these from video-level comparisons.
- Repeated records exist. The `index` column is a row identifier and should not be used as an analysis variable.
- The data file is not stored in this repository. In the current workspace, `youtube.csv` is one directory above this project folder.

## Objectives

1. Describe the dataset and check its data quality.
2. Prepare a consistent analysis table with valid video-country observations.
3. Measure how views relate to likes and comments.
4. Summarize the findings and limitations, then suggest cautious follow-up questions.

## Notebook outline

1. Title, summary, and table of contents
2. Import libraries
3. View the data
4. Clean and prepare the analysis dataframe
5. Check data quality and complete any needed cleaning
6. Visualize the data
7. Summarize findings and recommendations
