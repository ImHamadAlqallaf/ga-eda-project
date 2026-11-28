# LARP Guide (For Hamad)

A quick, plain-English cheat sheet so you can understand and present the whole project without getting stuck on technical details.

---

## 1. The Whole Project in 30 Seconds

- **Dataset:** 161,470 YouTube Trending records across 4 countries (US, Canada, France, Great Britain) from **November 14, 2017 to June 14, 2018** (`data/youtube.csv`).
- **The Problem:** We see view counts from 223 to 424.5 million, but we do not clearly know how **views** relate to **likes** and **comments** among trending videos. Also, messy rows (broken `#NAME?` video IDs and the same video repeated every day it stays trending) can skew the numbers.
- **What We Did:** Cleaned the data so each video is counted **once per country** (on its first trending day), checked how views relate to likes, comments, countries, and publish dates, and saved the charts into `charts/` for the slides.
- **The Golden Rule:** Everything we found shows **relationships**, not **causes**. We never claim that getting more likes or publishing on a certain day *causes* more views.

---

## 2. How the Notebook Flows (Part by Part)

### Part 1 & 2: Load and Inspect the Data
- Loaded `data/youtube.csv` (`161,470` rows, `18` columns).
- Checked `describe()` on the raw numbers: average views (~2.42M) are way higher than the median (~385K) because a few massive videos pull the average up. That is why we rely on the **median** throughout the project.

### Part 3: Data Quality Checks
- **Missing values:** 0 nulls across all 18 columns (confirmed with `missingno`).
- **Exact duplicates:** 219 rows (ignoring `index`).
- **Broken IDs:** 1,799 rows have `#NAME?` as `video_id` (happens when Excel breaks IDs starting with `-` or `=`).
- **Disabled flags:** 2,788 rows have comments disabled and 1,424 rows have ratings disabled. Those rows are **not** zero engagement—people simply could not click like or comment.

### Part 4: Cleaning (`newyoutube_df`)
1. Dropped `index`, removed the 219 exact duplicates, and removed the 1,799 `#NAME?` rows (`2,016` rows removed in total, leaving `159,454`).
2. Converted `trending_date` (`%y.%d.%m`) and `publish_date` (`%d/%m/%Y`) using `pd.to_datetime(..., format=...)` because `astype()` fails on `17.14.11` and swaps day/month on `12/11/2017`.
3. Checked the trending dates: 205 unique dates out of 213 days (missing **Jan 10–11** and **Apr 8–13, 2018**).
4. Kept only the **first trending record** for each `(video_id, publish_country)` pair so multi-day trending videos are not counted over and over.
   - **Clean table size:** **63,801 records**.
   - **Clean median:** **124,496 views**, **2,727 likes**, **408 comments**.

### Part 5: Views, Likes, and Comments (`engagement_df`)
- Filtered out rows where ratings or comments were disabled (`62,155` rows left).
- **Views distribution (`charts/views_under_1m.png`):** About **91%** of trending videos had under 1 million views on their first trending day. You do not need millions of views to trend.
- **Correlation heatmap (`charts/correlation_heatmap.png`):**
  - `views` and `likes`: **0.71** (fairly strong—more views usually means more likes).
  - `views` and `comment_count`: **0.53** (weaker—comments vary more).
  - `likes` and `comment_count`: **0.82** (strong—videos with many likes also get many comments).
  - `views` and `dislikes`: **0.41** (weakest).
- **Engagement rates:** Median video got **2.75 likes** and **0.38 comments** per 100 views.
- **Countries (`charts/median_views_by_country.png`):**
  - France: **30,181** videos, median **~45K** views.
  - Canada: **24,103** videos, median **~221K** views.
  - US: **6,281** videos, median **~271K** views.
  - GB: **3,236** videos, median **~279K** views.
  - Because France makes up almost half the clean table with lower views, it pulls the overall median down.

### Part 6: Publish Dates and Performance
- **Days to trend (`charts/days_to_trend.png`):** **49,047 of 63,801 (~77%)** first trended **1 day** after publishing.
- **Daily publishing & views (`charts/daily_publish_and_views.png`):** Most days had 250–350 newly trending videos. The dip in early April matches the missing trending-date gap (Apr 8–13), not fewer uploads. Median views rose from ~100K to ~200K between March and June 2018.
- **Busy vs quiet days scatter plot:** Once you ignore the data-gap days, there is no clear link between how many videos were published on a day and median views.

### Part 7: Recommendations & Illustrative Example
- **Illustrative chart (`charts/illustrative_likes_scenario.png`):** Not a prediction. It takes the dataset's median views (`124,496`) and multiplies by our real 25th, 50th, and 75th percentile like rates:
  - Low (bottom 25%): **~1,172 likes**
  - Typical (median): **~3,422 likes**
  - High (top 25%): **~7,569 likes**
- **Key takeaways to say in the presentation:**
  1. Use **likes per 100 views** (~1 to 6 is typical) rather than raw likes to compare videos.
  2. Check engagement in the **first 1–2 days**, since ~77% trend the next day.
  3. Always report **medians**, not averages.
  4. Look at **countries separately** (France vs US/GB/Canada).
  5. Do **not** recommend a "best publish day" from this dataset—the data does not support it.

---

## 3. Presentation Cheat Sheet (`youtube_trending_eda.pptx`)

A 10-slide PowerPoint is saved in the repo as `youtube_trending_eda.pptx` (using our `charts/` PNGs):

- **Slide 1 (Title):** Project title, Ali AlRashedi & Hamad Alqallaf.
- **Slide 2 (Problem Statement):** Unclear relationship between `views` and `likes`/`comments` across 161,470 trending records; messy `#NAME?` IDs and repeated daily rows.
- **Slide 3 (Objectives):** Check quality -> Clean to 63,801 first-day records -> Measure relationships -> Summarise & recommend.
- **Slide 4 & 5 (Finding 1):** Likes follow views (`0.71`) more closely than comments (`0.53`). Median is `2.75` likes and `0.38` comments per 100 views. Explain correlation ≠ causation.
- **Slide 6 & 7 (Finding 2):** `90.5%` of trending videos start under 1M views (median `124,496` vs average `436,533`). Say why we use medians and split by country.
- **Slide 8 & 9 (Finding 3):** `76.9%` (`49,047` videos) trend 1 day after publishing (`93.1%` within 2 days). Mention missing dates (`Jan 10–11`, `Apr 8–13`) mean no "best publish day".
- **Slide 10 (Conclusion & Recommendations):** Use likes per 100 views (~1–6), track days 1–2, report medians, and analyse countries separately.

---

## 4. Files in This Branch (`ali`)

- `README.md` — Project overview, problem statement, objectives, findings, recommendations, and To-Do list.
- `youtube_trending_analysis.ipynb` — Full executed notebook (all 107 cells, zero errors).
- `youtube_trending_eda.pptx` — 10-slide PowerPoint presentation.
- `charts/` — 6 saved PNG charts ready for slides.
- `data/youtube.csv` — Original dataset (untouched).
- `LARP.md` — This guide.
