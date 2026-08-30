# 🎵 Spotify Data Analysis using SQL

## 📌 Project Overview

This project is a **SQL-based Exploratory Data Analysis (EDA) project** performed on a Spotify dataset.

The objective of this project is to analyze songs, artists, albums, Spotify streams, YouTube performance, and audio characteristics using SQL.

The project starts with basic data exploration and gradually moves toward more advanced SQL concepts such as:

* Aggregate Functions
* Filtering
* Grouping
* Sorting
* Conditional Aggregation
* Subqueries
* Common Table Expressions (CTEs)
* Window Functions
* `DENSE_RANK()`
* Data Cleaning

---

# 🎧 About the Dataset

The dataset contains information about music tracks along with their Spotify and YouTube performance.

Each row represents information about a track, including:

* Artist and track information
* Album information
* Audio characteristics
* YouTube performance
* Spotify streams
* Licensing information
* Official video information
* Platform popularity

The dataset combines **music/audio features** with **social-media/video engagement metrics**, making it useful for both technical SQL practice and business-oriented analysis.

---

# 📊 Dataset Columns

| Column             | Description                                          |
| ------------------ | ---------------------------------------------------- |
| `artist`           | Name of the artist                                   |
| `track`            | Name of the track                                    |
| `album`            | Name of the album                                    |
| `album_type`       | Type of album, such as album or single               |
| `danceability`     | Measures how suitable a track is for dancing         |
| `energy`           | Represents the intensity and activity of a track     |
| `loudness`         | Overall loudness of the track                        |
| `speechiness`      | Detects the presence of spoken words                 |
| `acousticness`     | Confidence that the track is acoustic                |
| `instrumentalness` | Predicts whether a track contains vocals             |
| `liveness`         | Detects the presence of an audience in the recording |
| `valence`          | Musical positivity or mood of the track              |
| `tempo`            | Estimated tempo of the track in BPM                  |
| `duration_min`     | Track duration in minutes                            |
| `title`            | YouTube video title                                  |
| `channel`          | YouTube channel                                      |
| `views`            | Number of YouTube views                              |
| `likes`            | Number of YouTube likes                              |
| `comments`         | Number of YouTube comments                           |
| `licensed`         | Whether the content is licensed                      |
| `official_video`   | Whether the YouTube video is an official video       |
| `stream`           | Number of streams                                    |
| `energy_liveness`  | Combined energy and liveness metric                  |
| `most_played_on`   | Platform where the track is most played              |

---

# 🗄️ 1. Table Creation

```sql
DROP TABLE IF EXISTS spotify;

CREATE TABLE spotify (
    artist VARCHAR(255),
    track VARCHAR(255),
    album VARCHAR(255),
    album_type VARCHAR(50),
    danceability FLOAT,
    energy FLOAT,
    loudness FLOAT,
    speechiness FLOAT,
    acousticness FLOAT,
    instrumentalness FLOAT,
    liveness FLOAT,
    valence FLOAT,
    tempo FLOAT,
    duration_min FLOAT,
    title VARCHAR(255),
    channel VARCHAR(255),
    views FLOAT,
    likes BIGINT,
    comments BIGINT,
    licensed BOOLEAN,
    official_video BOOLEAN,
    stream BIGINT,
    energy_liveness FLOAT,
    most_played_on VARCHAR(50)
);
```

### Explanation

The query creates a table named `spotify` and defines the columns and their respective data types.

---

# 🔍 2. Initial Data Exploration

## 2.1 Display all records

```sql
SELECT *
FROM spotify;
```

### Purpose

Displays the complete Spotify dataset.

This is useful for understanding:

* Dataset structure
* Available columns
* Data values
* Potential data-quality issues

---

## 2.2 Count total records

```sql
SELECT COUNT(*)
FROM spotify;
```

### Purpose

Returns the total number of records in the dataset.

### SQL Concept

`COUNT()` is an aggregate function used to count rows.

---

## 2.3 Count distinct artists

```sql
SELECT COUNT(DISTINCT artist)
FROM spotify;
```

### Purpose

Returns the total number of unique artists in the dataset.

### SQL Concepts

* `COUNT()`
* `DISTINCT`

---

## 2.4 Find distinct album types

```sql
SELECT DISTINCT album_type
FROM spotify;
```

### Purpose

Returns all unique album types present in the dataset.

This helps understand the distribution of different types of releases.

---

## 2.5 Find maximum track duration

```sql
SELECT MAX(duration_min)
FROM spotify;
```

### Purpose

Returns the longest track duration in minutes.

### SQL Concept

`MAX()` returns the highest value from a column.

---

## 2.6 Find minimum track duration

```sql
SELECT MIN(duration_min)
FROM spotify;
```

### Purpose

Returns the shortest track duration in minutes.

### SQL Concept

`MIN()` returns the lowest value from a column.

---

# 🧹 3. Data Cleaning

## 3.1 Delete tracks with zero duration

```sql
DELETE FROM spotify
WHERE duration_min = 0;
```

### Purpose

Removes records where the track duration is `0`.

Zero-duration records can represent invalid or incomplete data.

---

## 3.2 Check whether zero-duration records remain

```sql
SELECT *
FROM spotify
WHERE duration_min = 0;
```

### Purpose

Verifies whether any zero-duration records are still present after deletion.

### Expected Result

Ideally, this query should return **no rows** after the deletion.

---

# 📺 4. Categorical Data Analysis

## 4.1 Find distinct YouTube channels

```sql
SELECT DISTINCT channel
FROM spotify;
```

### Purpose

Returns all unique YouTube channels available in the dataset.

---

## 4.2 Find the platforms where tracks are most played

```sql
SELECT DISTINCT most_played_on
FROM spotify;
```

### Purpose

Returns the unique values available in the `most_played_on` column.

This can be used to identify platforms such as:

* Spotify
* YouTube

---

# 📈 5. Data Analysis Questions

## Q1. Retrieve tracks with more than 1 billion streams

```sql
SELECT *
FROM spotify
WHERE stream > 1000000000;
```

### Purpose

Finds tracks that have more than **1 billion streams**.

### Business Insight

These tracks represent highly successful songs based on streaming performance.

### SQL Concept

`WHERE` is used to filter records based on a condition.

---

# 💿 Q2. List all albums along with their respective artists

```sql
SELECT DISTINCT
    album,
    artist
FROM spotify
ORDER BY album;
```

### Purpose

Returns unique album and artist combinations.

### Why `DISTINCT`?

An album can contain multiple tracks. Without `DISTINCT`, the same album and artist could appear multiple times.

### SQL Concepts

* `DISTINCT`
* `ORDER BY`

---

# 💬 Q3. Calculate total comments for licensed tracks

```sql
SELECT
    SUM(comments) AS total_comments
FROM spotify
WHERE licensed = TRUE;
```

### Purpose

Calculates the total number of comments for tracks where the content is licensed.

### SQL Concepts

* `SUM()`
* `WHERE`
* Boolean filtering

### Checking the `licensed` column

```sql
SELECT DISTINCT licensed
FROM spotify;
```

This can be used to verify the values stored in the Boolean column.

---

# 🎵 Q4. Find all tracks that belong to the album type "single"

```sql
SELECT *
FROM spotify
WHERE album_type = 'single';
```

### Purpose

Returns all tracks that are classified as singles.

### SQL Concept

Filtering using `WHERE`.

---

# 👨‍🎤 Q5. Count the total number of tracks by each artist

```sql
SELECT
    artist,
    COUNT(*) AS total_no_songs
FROM spotify
GROUP BY artist
ORDER BY total_no_songs DESC;
```

### Purpose

Counts how many tracks are associated with each artist.

### Result Interpretation

Artists are sorted from the artist with the **highest number of tracks to the lowest**.

### SQL Concepts

* `COUNT()`
* `GROUP BY`
* `ORDER BY`
* `DESC`

---

# 💃 Q6. Calculate the average danceability of tracks in each album

```sql
SELECT
    album,
    AVG(danceability) AS avg_danceability
FROM spotify
GROUP BY album
ORDER BY avg_danceability DESC;
```

### Purpose

Calculates the average danceability score for each album.

### Result Interpretation

Albums appearing at the top have higher average danceability.

### SQL Concept

`AVG()` is used to calculate the mean value.

---

# ⚡ Q7. Find the top 5 tracks with the highest energy values

The original query was incomplete:

```sql
SELECT
    track,
    MAX(energy)
```

A complete version is:

```sql
SELECT
    track,
    MAX(energy) AS max_energy
FROM spotify
GROUP BY track
ORDER BY max_energy DESC
LIMIT 5;
```

### Purpose

Finds the five tracks with the highest energy values.

### SQL Concepts

* `MAX()`
* `GROUP BY`
* `ORDER BY`
* `LIMIT`

---

# ▶️ Q8. Find tracks with official videos and their views and likes

The original query contains a small mistake:

```sql
SUM(views) AS total_likes
```

The correct version is:

```sql
SELECT
    track,
    SUM(views) AS total_views,
    SUM(likes) AS total_likes
FROM spotify
WHERE official_video = TRUE
GROUP BY track
ORDER BY total_views DESC
LIMIT 5;
```

### Purpose

Finds the top 5 tracks with official videos based on YouTube views.

The query returns:

* Track name
* Total views
* Total likes

### Important Correction

`likes` should be summed for `total_likes`, not `views`.

Correct:

```sql
SUM(likes) AS total_likes
```

---

# 👀 Q9. Calculate total views for tracks associated with each album

Your original query is:

```sql
SELECT
    album,
    track,
    SUM(views)
FROM spotify
GROUP BY album, track
ORDER BY 3 DESC;
```

### Purpose

This query calculates total views for each **album-track combination**.

A cleaner version is:

```sql
SELECT
    album,
    track,
    SUM(views) AS total_views
FROM spotify
GROUP BY album, track
ORDER BY total_views DESC;
```

### Important Note

If the question is strictly:

> Calculate the total views of all tracks for each album

then the query should be:

```sql
SELECT
    album,
    SUM(views) AS total_views
FROM spotify
GROUP BY album
ORDER BY total_views DESC;
```

The second version gives the total views at the **album level**.

---

# 🎧 Q10. Find tracks streamed more on Spotify than YouTube

```sql
SELECT *
FROM
(
    SELECT
        track,

        COALESCE(
            SUM(
                CASE
                    WHEN most_played_on = 'Youtube'
                    THEN stream
                END
            ),
            0
        ) AS streamed_on_youtube,

        COALESCE(
            SUM(
                CASE
                    WHEN most_played_on = 'Spotify'
                    THEN stream
                END
            ),
            0
        ) AS streamed_on_spotify

    FROM spotify
    GROUP BY track
) AS t1

WHERE streamed_on_spotify > streamed_on_youtube
  AND streamed_on_youtube <> 0;
```

### Purpose

Finds tracks where:

```text
Spotify Streams > YouTube Streams
```

---

## How the query works

### Step 1 — Group by track

```sql
GROUP BY track
```

Creates one result for each track.

---

### Step 2 — Separate YouTube streams

```sql
CASE
    WHEN most_played_on = 'Youtube'
    THEN stream
END
```

Only includes streams associated with YouTube.

---

### Step 3 — Separate Spotify streams

```sql
CASE
    WHEN most_played_on = 'Spotify'
    THEN stream
END
```

Only includes Spotify streams.

---

### Step 4 — Handle NULL values

```sql
COALESCE(..., 0)
```

Converts `NULL` into `0`.

---

### Step 5 — Compare platforms

```sql
WHERE streamed_on_spotify > streamed_on_youtube
```

Returns only tracks where Spotify has more streams than YouTube.

---

# 🏆 6. Hard SQL Queries

The following queries use more advanced SQL concepts.

---

# Q11. Find the top 3 most-viewed tracks for each artist

```sql
WITH ranking_artist AS
(
    SELECT
        artist,
        track,
        SUM(views) AS total_view,

        DENSE_RANK() OVER (
            PARTITION BY artist
            ORDER BY SUM(views) DESC
        ) AS rank

    FROM spotify
    GROUP BY artist, track
)

SELECT *
FROM ranking_artist
WHERE rank <= 3;
```

### Purpose

Finds the **top 3 most-viewed tracks for every artist**.

---

## How it works

### Step 1 — Create a CTE

```sql
WITH ranking_artist AS (...)
```

The CTE temporarily stores the calculated result.

---

### Step 2 — Calculate total views

```sql
SUM(views) AS total_view
```

Calculates total views for each track.

---

### Step 3 — Group by artist and track

```sql
GROUP BY artist, track
```

Creates an artist-track level aggregation.

---

### Step 4 — Partition by artist

```sql
PARTITION BY artist
```

Creates a separate ranking for each artist.

For example:

```text
Artist A → Rank tracks
Artist B → Rank tracks
Artist C → Rank tracks
```

---

### Step 5 — Rank tracks

```sql
DENSE_RANK() OVER (
    PARTITION BY artist
    ORDER BY SUM(views) DESC
)
```

The track with the highest views gets rank `1`.

---

### Step 6 — Keep top 3

```sql
WHERE rank <= 3
```

Returns only the top three ranked tracks for each artist.

### SQL Concepts

* CTE
* Window Function
* `DENSE_RANK()`
* `PARTITION BY`
* Aggregation

---

# 📊 Q12. Find tracks where liveness is above the average

```sql
SELECT
    track,
    artist,
    liveness
FROM spotify
WHERE liveness >
(
    SELECT AVG(liveness)
    FROM spotify
);
```

### Purpose

Finds tracks whose liveness score is greater than the average liveness across the dataset.

---

## How it works

### Inner Query

```sql
SELECT AVG(liveness)
FROM spotify;
```

Calculates the average liveness.

### Outer Query

```sql
WHERE liveness > (...)
```

Returns only tracks whose liveness is above that average.

### SQL Concept

This is an example of a **subquery**.

---

# ⚙️ Q13. Find the difference between the highest and lowest energy values for each album

```sql
WITH cte AS
(
    SELECT
        album,
        MAX(energy) AS highest_energy,
        MIN(energy) AS lowest_energy
    FROM spotify
    GROUP BY album
)

SELECT
    album,
    highest_energy - lowest_energy AS energy_diff
FROM cte
ORDER BY energy_diff DESC;
```

### Purpose

Calculates the difference between the highest and lowest energy values for each album.

### Formula

```text
Energy Difference =
Highest Energy - Lowest Energy
```

---

## How it works

### Step 1 — Calculate maximum energy

```sql
MAX(energy)
```

Finds the highest energy value in each album.

---

### Step 2 — Calculate minimum energy

```sql
MIN(energy)
```

Finds the lowest energy value in each album.

---

### Step 3 — Calculate the difference

```sql
highest_energy - lowest_energy
```

Measures the variation in energy levels within each album.

---

### Interpretation

A higher `energy_diff` means that the album contains tracks with a wider range of energy levels.

### SQL Concepts

* CTE
* `MAX()`
* `MIN()`
* `GROUP BY`
* Calculated columns
* `ORDER BY`

---

# 🧠 7. SQL Concepts Practiced

| SQL Concept     | Where Used                            |
| --------------- | ------------------------------------- |
| `SELECT`        | Data retrieval                        |
| `DISTINCT`      | Unique artists, albums and categories |
| `WHERE`         | Filtering                             |
| `COUNT()`       | Counting records                      |
| `SUM()`         | Streams, views, likes and comments    |
| `AVG()`         | Danceability and liveness             |
| `MAX()`         | Duration and energy                   |
| `MIN()`         | Duration and energy                   |
| `GROUP BY`      | Artist, album and track analysis      |
| `ORDER BY`      | Sorting results                       |
| `LIMIT`         | Top 5 analysis                        |
| `CASE WHEN`     | Spotify vs YouTube comparison         |
| `COALESCE`      | Handling NULL values                  |
| Subquery        | Above-average liveness                |
| CTE             | Energy analysis and ranking           |
| Window Function | Ranking tracks                        |
| `DENSE_RANK()`  | Top tracks per artist                 |
| `PARTITION BY`  | Ranking within each artist            |

---

# 💼 8. Business Questions Answered

This project uses SQL to answer practical music-industry questions.

### Streaming Performance

* Which tracks have more than 1 billion streams?
* Which tracks perform better on Spotify than YouTube?
* Which tracks have the highest YouTube views?

### Artist Analysis

* Which artists have the highest number of tracks?
* What are the top 3 most-viewed tracks for each artist?

### Album Analysis

* Which albums have the highest average danceability?
* Which albums generate the most views?
* Which albums have the largest variation in energy?

### YouTube Analysis

* Which tracks have official videos?
* Which tracks have the highest views?
* How many likes and comments are generated?

### Audio Analysis

* Which tracks have the highest energy?
* Which tracks have above-average liveness?
* Which albums have the largest energy range?

---

# 📌 9. Key Insights From the Analysis

The SQL analysis is designed to identify:

1. **Highly successful tracks** based on streaming volume.
2. **Popular artists** based on the number of tracks.
3. **Highly danceable albums** based on average danceability.
4. **High-energy tracks** using the energy feature.
5. **High-performing YouTube videos** using views and likes.
6. **Platform differences** between Spotify and YouTube.
7. **Top-performing tracks within each artist** using window functions.
8. **Tracks with higher-than-average liveness** using a subquery.
9. **Albums with the widest energy variation** using a CTE.

---

# 🧹 10. Data Cleaning Performed

One data-cleaning step was performed:

```sql
DELETE FROM spotify
WHERE duration_min = 0;
```

This removes tracks having a duration of zero.

The records were then checked using:

```sql
SELECT *
FROM spotify
WHERE duration_min = 0;
```

The purpose was to ensure that invalid zero-duration records were removed before analysis.

---

# 🛠️ 11. Technologies Used

* **SQL**
* **MySQL**
* **Spotify Dataset**

---

# 📁 12. Suggested GitHub Structure

```text
Spotify-SQL-Analysis/
│
└── README.md
```

Since this project is being presented as a SQL portfolio project, the README contains the complete analysis workflow and queries.

---

# 🎯 13. Project Learning Outcomes

Through this project, I practiced SQL from basic to advanced level.

### Basic SQL

* SELECT
* WHERE
* DISTINCT
* ORDER BY
* LIMIT

### Intermediate SQL

* GROUP BY
* COUNT
* SUM
* AVG
* MAX
* MIN
* CASE WHEN
* COALESCE

### Advanced SQL

* Subqueries
* CTEs
* Window Functions
* DENSE_RANK
* PARTITION BY

The project demonstrates how SQL can be used to transform raw music data into structured analysis and business-oriented insights.

---

# 👨‍💻 Author

**Harjot Singh**

### Project

**Spotify Data Analysis using SQL**

### Focus

**SQL | Data Analysis | Exploratory Data Analysis | Business Insights**
