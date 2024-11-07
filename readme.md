# Spotify Data Analysis Project in SQL

### 1. Database Setup

```sql
CREATE TABLE IF NOT EXISTS spotify (
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

### 2. Data Cleaning & Exploration

```sql
SELECT COUNT(*) FROM spotify;

SELECT COUNT(DISTINCT artist) FROM spotify;

SELECT COUNT(DISTINCT album) FROM spotify;

SELECT DISTINCT album_type FROM spotify;

SELECT MAX(duration_min) FROM spotify;

SELECT MIN(duration_min) FROM spotify;

SELECT * FROM spotify
WHERE duration_min = 0;

DELETE FROM spotify
WHERE duration_min = 0;

SELECT * FROM spotify
WHERE duration_min = 0;

SELECT DISTINCT channel FROM spotify;

SELECT DISTINCT most_played_on FROM spotify;
```

### 3. Data Analysis

**Q1. Retrieve the names of all tracks that have more than 1 billion streams.**

```sql
SELECT track FROM spotify WHERE stream > 1000000000;
```

**Q2. List all albums along with their respective artists.**

```sql
SELECT DISTINCT album, artist FROM spotify;
```

**Q3. Get the total number of comments for tracks where licensed = TRUE.**

```sql
SELECT SUM(comments) as total_comments
FROM spotify
WHERE licensed='TRUE';
```

**Q4. Find all tracks that belong to the album type single.**

```sql
SELECT * FROM spotify
WHERE album_type = 'single';
```

**Q5. Count the total number of tracks by each artist.**

```sql
SELECT artist, COUNT(track) as no_of_tracks
FROM spotify
GROUP BY artist
ORDER BY no_of_tracks DESC;
```

**Q6. Calculate the average danceability of tracks in each album.**

```sql
SELECT album, AVG(danceability) as avg_danceability
FROM spotify
GROUP BY album
ORDER BY avg_danceability DESC;
```

**Q7. Find the top 5 tracks with the highest energy values.**

```sql
SELECT track, MAX(energy)
FROM spotify
GROUP BY track
ORDER BY 2 DESC
LIMIT 5;
```

**Q8. List all tracks along with their views and likes where official_video = TRUE.**

```sql
SELECT track,
SUM(views) as total_views,
SUM(likes) as total_likes
FROM spotify
WHERE official_video = 'TRUE'
GROUP BY track
ORDER BY total_views DESC;
```

**Q9. For each album, calculate the total views of all associated tracks.**

```sql
SELECT album, track, SUM(views)
FROM spotify
GROUP BY album, track
ORDER BY 3 DESC;
```

**Q10. Find the top 3 most-viewed tracks for each artist using window functions.**

```sql
WITH ranking_artist
AS
(
	SELECT artist, track, SUM(views) as total_views,
	DENSE_RANK() OVER(PARTITION BY artist ORDER BY SUM(views) DESC) as rank
	FROM spotify
	GROUP BY 1, 2
	ORDER BY 1, 3 DESC
)
SELECT * FROM ranking_artist
WHERE rank <= 3;
```
