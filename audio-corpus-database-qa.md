# 🎧 QA: Creating an Audio Corpus Database

This QA document outlines how to design and build an audio corpus database, including schema structure, data handling, metadata management, and best practices for storage and retrieval.

---

## ✅ General Overview

**Q1: What is an audio corpus database?**  
**A:** A database designed to store, manage, and query collections of audio recordings and their associated metadata or annotations.

**Q2: What are the common applications of an audio corpus database?**  
**A:**  
- Speech recognition model training  
- Speaker identification  
- Acoustic scene classification  
- Sound event detection  
- Audio dataset management for ML

---

## 🗃️ Database Schema Design

**Q3: What are the core tables in an audio corpus database?**  
**A:**  
- `audio_files`: basic info and path to audio  
- `annotations`: timestamped labels or transcripts  
- `speakers`: speaker information  
- `recording_sessions`: metadata like device, location  
- `projects`: to organize datasets into groups

**Q4: Example schema for `audio_files` table:**  
```sql
audio_files (
  id BIGINT PRIMARY KEY,
  file_path TEXT,
  duration FLOAT,
  sample_rate INT,
  format VARCHAR(10),
  session_id BIGINT,
  speaker_id BIGINT,
  upload_time TIMESTAMP
)
```

---

## 🔁 Relationships and Metadata

**Q5: How do you model many-to-one relationships (e.g., audio to speaker)?**  
**A:** Use foreign keys to reference a `speakers` table from the `audio_files` table.

**Q6: How do you store annotations like transcripts or sound events?**  
```sql
annotations (
  id BIGINT PRIMARY KEY,
  audio_id BIGINT,
  label TEXT,
  start_time FLOAT,
  end_time FLOAT,
  type ENUM('word', 'event', 'phoneme'),
  confidence FLOAT
)
```

---

## 📂 Data Handling

**Q7: Should audio files be stored in the database?**  
**A:** No. Store file paths or URLs pointing to external storage (e.g., local disk, S3, GCS).

**Q8: How to ensure integrity of audio files?**  
**A:**  
- Store file hash (e.g., SHA256)  
- Use checksums during upload  
- Validate file format and length

---

## 🧩 Annotation & Versioning

**Q9: How to manage multiple annotation versions?**  
**A:** Add a `version` or `annotation_set_id` field to track versions or group annotations.

**Q10: How to support multiple annotators?**  
```sql
annotations (
  ...
  annotator_id BIGINT,
  created_at TIMESTAMP
)
```

---

## 📊 Query Examples

**Q11: Get all audio files recorded at 16kHz with duration > 5 seconds:**  
```sql
SELECT * FROM audio_files
WHERE sample_rate = 16000 AND duration > 5.0;
```

**Q12: Find all annotations of type 'event' for a given audio:**  
```sql
SELECT * FROM annotations
WHERE audio_id = 42 AND type = 'event';
```

---

## 🛠 Best Practices

**Q13: How to organize large corpora?**  
**A:**  
- Use UUIDs for IDs  
- Partition by project, date, or session  
- Index common filters (speaker_id, duration, date)  
- Use views or materialized views for fast retrieval

**Q14: What tools help build or query an audio corpus database?**  
**A:**  
- PostgreSQL, SQLite, or MongoDB  
- SQLAlchemy, Django ORM  
- Pandas, Apache Superset for analysis

---

## 🧠 Bonus Tips

- Normalize speaker names, device types  
- Support tagging for flexible search  
- Implement role-based access for annotators and admins  
- Back up metadata and store file hashes for validation  
- Use audio fingerprints or hashes to detect duplicates

