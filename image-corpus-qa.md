# 🧠 QA: Image Corpus Database Modeling

This document provides common questions and answers related to designing and modeling an image corpus database. It can be used as a reference or evaluation material in programming contests, interviews, or educational settings.

---

## ✅ General Concepts

**Q1: What is an image corpus database?**  
**A:** It is a structured collection of image data and associated metadata, typically used for machine learning, computer vision, or research tasks.

**Q2: What are the core entities in an image corpus database?**  
**A:** 
- `Image`: The actual image file or reference.
- `Label`: Annotation or tag assigned to the image.
- `Category`: Group or class to which the image belongs.
- `Source`: Where the image was obtained (e.g., URL, user, device).
- `Annotation`: Detailed data like bounding boxes, segmentation, etc.

---

## 📦 Basic Schema Design

**Q3: What are the essential fields in the `Image` table?**  
**A:**
```sql
Image (
  id BIGINT PRIMARY KEY,
  file_path TEXT,
  file_hash TEXT UNIQUE,
  width INT,
  height INT,
  format VARCHAR(10),
  upload_date TIMESTAMP,
  source_id BIGINT
)
```

**Q4: How should image labels be modeled?**  
**A:** Using a many-to-many relationship:
```sql
ImageLabel (
  image_id BIGINT,
  label_id BIGINT,
  PRIMARY KEY (image_id, label_id)
)
```

**Q5: What are common metadata fields for labeling?**  
**A:** Label name, label type (classification, object detection), confidence score, annotator ID, and timestamp.

---

## 🧩 Advanced Modeling

**Q6: How to store annotations like bounding boxes?**  
**A:**
```sql
Annotation (
  id BIGINT PRIMARY KEY,
  image_id BIGINT,
  label_id BIGINT,
  x INT,
  y INT,
  width INT,
  height INT,
  annotator_id BIGINT,
  created_at TIMESTAMP
)
```

**Q7: How to handle duplicate or similar images?**  
**A:** Use `file_hash` or perceptual hash (pHash) to detect duplicates. Store similarity score in a separate table if needed.

---

## 📁 Storage and Access

**Q8: Should images be stored in the database?**  
**A:** It's generally better to store image paths or URLs pointing to a file storage system (e.g., S3, GCS) instead of BLOBs in the DB.

**Q9: How to manage image versioning?**  
**A:** Add a `version` column and track changes over time using a `history` or `revision` table.

**Q10: How can access to image data be controlled?**  
**A:** Use a user-role table and access policies per image/project.

---

## 🔍 Query Examples

**Q11: Find all images labeled as "cat":**
```sql
SELECT i.*
FROM Image i
JOIN ImageLabel il ON i.id = il.image_id
JOIN Label l ON il.label_id = l.id
WHERE l.name = 'cat';
```

**Q12: Get all bounding boxes for a specific image:**
```sql
SELECT x, y, width, height, label_id
FROM Annotation
WHERE image_id = 123;
```

---

## 🛠 Tools and Best Practices

**Q13: What tools are commonly used for labeling?**  
**A:** CVAT, Labelbox, MakeSense.ai, Supervisely.

**Q14: What database is recommended for large image corpora?**  
**A:** PostgreSQL (with PostGIS for spatial support), or NoSQL options like MongoDB for more flexible schemas.

**Q15: How to optimize performance for large-scale image corpora?**  
**A:** 
- Index fields like `label_id`, `image_id`, `created_at`
- Use a CDN for serving images
- Partition tables if needed

---

## 🏁 Bonus Tips

- Consider using UUIDs for image IDs if working in distributed systems.
- Use normalization for tags, categories, and annotation types.
- Store EXIF data or OCR output in a separate metadata table if required.
