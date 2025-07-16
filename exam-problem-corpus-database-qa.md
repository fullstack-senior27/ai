# 📝 QA: Creating an Exam Problem Corpus Database

This QA document guides the design and implementation of a database for storing and managing exam problems. It includes schema structure, data normalization, metadata management, and querying strategies.

---

## ✅ General Overview

**Q1: What is an exam problem corpus database?**  
**A:** A structured database designed to store, organize, and retrieve exam questions, choices, answers, tags, and related metadata.

**Q2: What are common use cases?**  
**A:**  
- Educational platforms  
- Exam preparation apps  
- Question recommendation engines  
- AI/ML model training for educational tasks

---

## 🗃️ Core Schema Design

**Q3: What are the essential tables in the database?**  
**A:**  
- `questions`: question text and metadata  
- `choices`: multiple-choice options  
- `answers`: correct answers  
- `subjects`: subject or topic classifications  
- `exams`: source exam info (e.g., year, name)

**Q4: Example schema for the `questions` table:**  
```sql
questions (
  id BIGINT PRIMARY KEY,
  question_text TEXT,
  difficulty ENUM('easy', 'medium', 'hard'),
  subject_id BIGINT,
  exam_id BIGINT,
  has_diagram BOOLEAN,
  created_at TIMESTAMP
)
```

---

## 🔢 Choices and Answers

**Q5: How to structure multiple-choice options?**  
```sql
choices (
  id BIGINT PRIMARY KEY,
  question_id BIGINT,
  label CHAR(1), -- e.g., 'A', 'B'
  text TEXT
)
```

**Q6: How to represent correct answers?**  
```sql
answers (
  question_id BIGINT,
  correct_choice_label CHAR(1),
  explanation TEXT
)
```

---

## 🔖 Metadata Tables

**Q7: What fields should the `subjects` table have?**  
```sql
subjects (
  id BIGINT PRIMARY KEY,
  name TEXT,
  parent_subject_id BIGINT NULL
)
```

**Q8: What is stored in the `exams` table?**  
```sql
exams (
  id BIGINT PRIMARY KEY,
  name TEXT,
  year INT,
  board TEXT
)
```

---

## 🔁 Relationships and Normalization

**Q9: How do you relate questions to subjects and exams?**  
**A:** Use foreign keys from `questions.subject_id` and `questions.exam_id`.

**Q10: Should tags or topics be normalized?**  
**A:** Yes. Use a many-to-many structure for flexible tagging:
```sql
question_tags (
  question_id BIGINT,
  tag_id BIGINT
)
tags (
  id BIGINT PRIMARY KEY,
  name TEXT
)
```

---

## 📂 Data Handling and Versioning

**Q11: Should questions be versioned?**  
**A:** Yes, add a `version` or maintain a history table with audit fields (`updated_by`, `updated_at`, etc.).

**Q12: How to handle images or diagrams in questions?**  
**A:** Store file paths or URLs in a separate `media` table linked to the question.

---

## 📊 Query Examples

**Q13: Get all 'Math' questions from 2022:**  
```sql
SELECT q.*
FROM questions q
JOIN subjects s ON q.subject_id = s.id
JOIN exams e ON q.exam_id = e.id
WHERE s.name = 'Math' AND e.year = 2022;
```

**Q14: Find all questions that include diagrams:**  
```sql
SELECT * FROM questions
WHERE has_diagram = TRUE;
```

---

## 🛠 Best Practices

**Q15: How to ensure data consistency?**  
**A:**  
- Use foreign keys and constraints  
- Validate input during ingestion  
- Deduplicate questions using hashes or similarity

**Q16: How to optimize for performance?**  
**A:**  
- Add indexes on `subject_id`, `exam_id`, `difficulty`  
- Use full-text search for question text  
- Use pagination for browsing large sets

---

## 🧩 Bonus Tips

- Normalize all labels and tags for consistency  
- Track user interactions for recommendation systems  
- Use UUIDs if working in distributed environments  
- Log edits or corrections with a moderation table

