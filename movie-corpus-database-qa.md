# 🎬 QA: Creating a Movie Corpus Database

This document contains essential questions and answers for designing and implementing a movie corpus database. It covers data structure, relationships, source handling, and best practices.

---

## ✅ Overview

**Q1: What is a movie corpus database?**  
**A:** A structured collection of movie-related data (e.g., titles, plots, genres, scripts, ratings) used for research, ML training, recommendation systems, or analytics.

**Q2: What are typical use cases for a movie corpus database?**  
**A:**  
- Movie recommendation engines  
- Genre classification or trend analysis  
- Sentiment analysis on reviews  
- Script-based NLP models

---

## 🗃️ Core Schema Design

**Q3: What are the essential tables in a movie corpus database?**  
**A:**  
- `movies`: Basic movie info  
- `genres`: List of genres  
- `movie_genres`: Many-to-many relation between movies and genres  
- `people`: Actors, directors, writers  
- `movie_people`: Roles of people in each movie  
- `scripts`: Dialogue or full script  
- `reviews`: User or critic reviews  
- `ratings`: Numerical ratings (IMDb, Rotten Tomatoes)

**Q4: Example schema for the `movies` table?**  
```sql
movies (
  id BIGINT PRIMARY KEY,
  title TEXT,
  release_year INT,
  runtime INT,
  language TEXT,
  country TEXT,
  summary TEXT
)
```

---

## 🔁 Relationships

**Q5: How do you model multi-genre movies?**  
**A:** Use a join table `movie_genres` with:
```sql
movie_genres (
  movie_id BIGINT,
  genre_id BIGINT,
  PRIMARY KEY (movie_id, genre_id)
)
```

**Q6: How do you model multiple roles for a person in a movie?**  
**A:** Use `movie_people` table with a role field:
```sql
movie_people (
  movie_id BIGINT,
  person_id BIGINT,
  role ENUM('Actor', 'Director', 'Writer'),
  character_name TEXT,
  PRIMARY KEY (movie_id, person_id, role)
)
```

---

## 📁 Scripts & Reviews

**Q7: Where to store movie scripts?**  
**A:** Store the script text in a separate `scripts` table with metadata like format, scene numbers, or speaker labels.

**Q8: How do you model user reviews?**  
```sql
reviews (
  id BIGINT PRIMARY KEY,
  movie_id BIGINT,
  user_id BIGINT,
  content TEXT,
  sentiment_score FLOAT,
  created_at TIMESTAMP
)
```

---

## 🗂️ Data Sources & Ingestion

**Q9: What are good sources for movie data?**  
**A:**  
- IMDb Datasets  
- TMDb API  
- MovieLens  
- Rotten Tomatoes  
- Subscene or IMSDb for scripts

**Q10: How to handle data from different formats (e.g., JSON, CSV, HTML)?**  
**A:** Use ETL scripts to:
- Normalize schema
- Convert encodings
- Clean null or inconsistent fields
- Schedule updates with cron jobs or Airflow

---

## 🛠 Best Practices

**Q11: Should you store media (e.g., posters, trailers) in the DB?**  
**A:** No, store only file paths or URLs to S3 or CDN.

**Q12: How to support full-text search on titles, plots, or scripts?**  
**A:** Use:
- PostgreSQL `tsvector`
- Elasticsearch for advanced search
- FTS (Full-Text Search) in SQLite for small datasets

---

## 📊 Query Examples

**Q13: Get all movies released after 2010 with the 'Drama' genre:**  
```sql
SELECT m.title
FROM movies m
JOIN movie_genres mg ON m.id = mg.movie_id
JOIN genres g ON mg.genre_id = g.id
WHERE g.name = 'Drama' AND m.release_year > 2010;
```

**Q14: Count number of movies per director:**  
```sql
SELECT p.name, COUNT(*) AS movie_count
FROM movie_people mp
JOIN people p ON mp.person_id = p.id
WHERE mp.role = 'Director'
GROUP BY p.name;
```

---

## 🧩 Bonus Tips

- Use UUIDs for distributed systems  
- Normalize tags and character names  
- Store timestamps for audit and history tracking  
- Create indices on common filters: genre, year, language

