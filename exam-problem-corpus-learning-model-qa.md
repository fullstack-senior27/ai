# 📝 QA: Exam Problem Corpus Learning Model

This QA document explores the design and development of learning models based on an exam problem corpus. It includes dataset preparation, feature engineering, model selection, and use cases for educational AI.

---

## ✅ General Overview

**Q1: What is an exam problem corpus?**  
**A:** A structured collection of exam questions, answers, topics, and metadata used to train machine learning models for educational tasks.

**Q2: What are typical applications of an exam problem learning model?**  
**A:**  
- Automatic question classification  
- Difficulty prediction  
- Question generation  
- Personalized recommendation  
- Plagiarism or similarity detection

---

## 📦 Dataset Design

**Q3: What are key elements in an exam problem dataset?**  
**A:**  
- Question text  
- Answer (single or multiple)  
- Choices (for MCQs)  
- Tags or topics  
- Difficulty level  
- Source (e.g., test name, year)

**Q4: How can questions be structured in a table?**  
```sql
questions (
  id BIGINT PRIMARY KEY,
  question_text TEXT,
  answer TEXT,
  choices JSONB,
  difficulty ENUM('easy', 'medium', 'hard'),
  subject TEXT,
  exam_name TEXT,
  year INT
)
```

---

## 🧪 Preprocessing

**Q5: How to preprocess exam problems for NLP tasks?**  
**A:**  
- Remove HTML/LaTeX formatting  
- Tokenize and normalize text  
- Encode answers and choices  
- Convert labels to integers or classes

**Q6: How to handle mathematical or symbolic content?**  
**A:**  
- Use MathML or LaTeX parsers  
- Extract features using symbolic representation (e.g., SymPy)  
- Train math-aware models (e.g., MathBERT)

---

## 🔍 Feature Engineering

**Q7: What features are useful for classification tasks?**  
**A:**  
- TF-IDF or BERT embeddings of question text  
- Number of words/sentences  
- Presence of images or formulas  
- POS tag patterns  
- Topic tags

**Q8: How to encode multiple-choice structure?**  
**A:**  
- Combine choices with question text  
- Represent choices as separate embeddings  
- Use special separator tokens in input (e.g., `[SEP]`)

---

## 🧠 Model Training

**Q9: What models are used for difficulty prediction or classification?**  
**A:**  
- Logistic Regression, Random Forest  
- CNN, LSTM  
- Transformers like BERT, RoBERTa  
- XGBoost for tabular + NLP hybrid features

**Q10: What models are used for question generation?**  
**A:**  
- T5 or GPT-style transformers  
- Seq2Seq LSTM with attention  
- Fine-tuned BART models

---

## 📈 Evaluation

**Q11: How to evaluate classification or tagging models?**  
**A:**  
- Accuracy, Precision, Recall, F1  
- Confusion Matrix  
- AUC for binary difficulty prediction

**Q12: How to evaluate question generation quality?**  
**A:**  
- BLEU, ROUGE, METEOR  
- Human evaluation for clarity, correctness  
- Diversity score (distinct n-grams)

---

## 🧪 Use Cases

**Q13: How to build a topic classifier for questions?**  
**A:**  
1. Preprocess and label questions with topics  
2. Convert text to embeddings (TF-IDF, BERT)  
3. Train classifier (e.g., Logistic Regression, BERT)  
4. Evaluate with F1-score

**Q14: How to predict question difficulty?**  
**A:**  
1. Engineer features: length, structure, topic  
2. Use labeled dataset with difficulty  
3. Train model (XGBoost or BERT classifier)  
4. Evaluate with classification metrics

---

## 🧩 Bonus Tips

- Use augmentation (paraphrasing) for low-data topics  
- Combine metadata + NLP features for hybrid models  
- Fine-tune pretrained models on domain-specific exam data  
- Use cosine similarity for duplicate question detection  
- Visualize embedding clusters by subject/topic

