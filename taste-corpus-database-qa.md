# 🍽️ QA: Creating a Taste Corpus Database

This QA document guides the design and implementation of a taste corpus database. It covers schema design, data structure, ingredient-flavor relationships, and metadata management.

---

## ✅ General Overview

**Q1: What is a taste corpus database?**  
**A:** A structured database that stores information about food or beverage items, their ingredients, flavors, sensory attributes, and user preferences.

**Q2: What are common use cases?**  
**A:**  
- Flavor prediction  
- Food recommendation systems  
- Sensory research  
- Recipe management and generation  
- Nutritional and taste profiling

---

## 🗃️ Core Schema Design

**Q3: What are essential tables in a taste corpus database?**  
**A:**  
- `items`: food or beverage entries  
- `ingredients`: list of ingredients  
- `item_ingredients`: many-to-many between items and ingredients  
- `flavors`: list of flavor tags  
- `item_flavors`: many-to-many between items and flavors  
- `users` and `ratings` (for personalized data)  
- `nutrition` and `preparation` (optional)

**Q4: Example schema for `items` table:**  
```sql
items (
  id BIGINT PRIMARY KEY,
  name TEXT,
  description TEXT,
  category TEXT,
  region TEXT,
  created_at TIMESTAMP
)
```

---

## 🌿 Ingredients & Flavors

**Q5: How to relate ingredients to items?**  
```sql
item_ingredients (
  item_id BIGINT,
  ingredient_id BIGINT,
  quantity TEXT,
  PRIMARY KEY (item_id, ingredient_id)
)
```

**Q6: How to model taste attributes?**  
```sql
flavors (
  id BIGINT PRIMARY KEY,
  name TEXT -- e.g., sweet, salty, umami
)

item_flavors (
  item_id BIGINT,
  flavor_id BIGINT,
  intensity FLOAT -- e.g., 0.0 to 1.0
)
```

---

## 🧪 Sensory & Metadata

**Q7: How to store sensory evaluation data?**  
```sql
sensory_profiles (
  item_id BIGINT,
  mouthfeel TEXT,
  aroma TEXT,
  aftertaste TEXT,
  panelist_id BIGINT,
  notes TEXT
)
```

**Q8: How to include nutrition or preparation data?**  
**A:** Use linked tables:
```sql
nutrition (
  item_id BIGINT,
  calories INT,
  sugar FLOAT,
  salt FLOAT,
  fat FLOAT
)

preparation (
  item_id BIGINT,
  instructions TEXT,
  cook_time_minutes INT
)
```

---

## 📊 Query Examples

**Q9: Find all items that are sweet and salty:**  
```sql
SELECT i.*
FROM items i
JOIN item_flavors f1 ON i.id = f1.item_id
JOIN item_flavors f2 ON i.id = f2.item_id
JOIN flavors fl1 ON f1.flavor_id = fl1.id
JOIN flavors fl2 ON f2.flavor_id = fl2.id
WHERE fl1.name = 'sweet' AND fl2.name = 'salty';
```

**Q10: Get ingredients and flavor profile for a specific item:**  
```sql
SELECT ing.name, fl.name, flv.intensity
FROM items i
JOIN item_ingredients ii ON i.id = ii.item_id
JOIN ingredients ing ON ii.ingredient_id = ing.id
JOIN item_flavors flv ON i.id = flv.item_id
JOIN flavors fl ON flv.flavor_id = fl.id
WHERE i.id = 123;
```

---

## 🛠 Best Practices

**Q11: How to manage ingredient and flavor normalization?**  
**A:**  
- Use canonical names (e.g., lowercase, singular)  
- Maintain ingredient synonyms table  
- Enforce controlled vocabularies for flavors

**Q12: How to ensure data integrity?**  
**A:**  
- Use foreign key constraints  
- Validate intensity ranges  
- Ensure uniqueness in many-to-many link tables

---

## 🧩 Bonus Tips

- Index flavor and ingredient names for fast lookup  
- Track region or cuisine for cultural context  
- Support tagging with user-submitted keywords  
- Use UUIDs if syncing across systems  
- Enable user ratings and notes for personalization

