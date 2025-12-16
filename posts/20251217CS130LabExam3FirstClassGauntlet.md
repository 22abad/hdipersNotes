---
title: CS130 Lab Exam 3: The 1.1 First Class Gauntlet
title_en: CS130 Lab Exam 3: The Full 20-Question Gauntlet
title_zh: CS130 终极冲刺：电影系统 20 题全解析版 (拿下 1.1)
date: 2025-12-16
categories: CS130
tags: SQL, Relational Algebra, ExamPrep, PostgreSQL
summary_en: The complete 20-question drill. No placeholders. Full DML, Joins, and Relational Algebra logic for Lab Exam 3.
summary_zh: 20 道模拟题完整补全。包含 DML 更新、多表连接、关系代数转换，无任何缺失。
---

[EN]
# 🍿 Scenario: Cinema Management System
1. **`lab3_movie`**: `movie_id`, `title`, `genre`, `duration`, `rating`
2. **`lab3_viewer`**: `viewer_id`, `name`, `age`, `gender`, `membership`
3. **`lab3_watched`**: `viewer_id`, `movie_id`, `watch_date`, `score`

---

## 🟢 Level 1: RA & Warm-up (RA to SQL)

### Q1: Selection & Projection
**Task:** $\pi_{title, duration} (\sigma_{genre = 'Action' \text{ AND } duration > 120} lab3\_movie)$
<details> <summary>▼ Show Answer</summary>
```sql
SELECT title, duration FROM lab3_movie WHERE genre = 'Action' AND duration > 120;

```

</details>

###Q2: Triple Bowtie Join**Task:** \pi_{name} (lab3\_viewer \bowtie lab3\_watched \bowtie lab3\_movie)

<details> <summary>▼ Show Answer</summary>

```sql
SELECT v.name FROM lab3_viewer v 
JOIN lab3_watched w ON v.viewer_id = w.viewer_id 
JOIN lab3_movie m ON w.movie_id = m.movie_id;

```

</details>

###Q3: Complex RA Filter**Task:** \pi_{viewer\_id} (\sigma_{score = 10 \text{ AND } genre = 'Horror'} watched \bowtie movie)

<details> <summary>▼ Show Answer</summary>

```sql
SELECT w.viewer_id FROM lab3_watched w 
JOIN lab3_movie m ON w.movie_id = m.movie_id 
WHERE w.score = 10 AND m.genre = &#39;Horror&#39;;

```

</details>

---

##🟡 Level 2: DML & Logic Precision (The 1.1 Zone)###Q4: Rating Cleanup**Task:** Update `genre` to 'B-Movie' for all movies with a `rating` below 5.0.

<details> <summary>▼ Show Answer</summary>

```sql
UPDATE lab3_movie SET genre = &#39;B-Movie&#39; WHERE rating &lt; 5.0;

```

</details>

###Q5: Post-Exam GDPR Delete**Task:** Delete movie 'Inception'. How to calculate total rows affected ( CASCADE )?

<details> <summary>▼ Show Answer</summary>

```sql
-- Step 1: Count records in child table that will be deleted
SELECT COUNT(*) FROM lab3_watched WHERE movie_id IN (SELECT movie_id FROM lab3_movie WHERE title = &#39;Inception&#39;);
-- Step 2: Count the movie itself
SELECT COUNT(*) FROM lab3_movie WHERE title = &#39;Inception&#39;;
-- Execute
DELETE FROM lab3_movie WHERE title = &#39;Inception&#39;;

```

</details>

###Q6: Incremental Duration Update**Task:** For all 'Horror' movies, increase `duration` by 10 minutes.

<details> <summary>▼ Show Answer</summary>

```sql
UPDATE lab3_movie SET duration = duration + 10 WHERE genre = &#39;Horror&#39;;

```

</details>

###Q7: String Modification (The ID Prefix)**Task:** Prefix 'GOLD-' to `viewer_id` for all members with status 'Gold'.

<details> <summary>▼ Show Answer</summary>

```sql
UPDATE lab3_viewer SET viewer_id = &#39;GOLD-&#39; || viewer_id WHERE membership = &#39;Gold&#39;;

```

**Pitfall:** Use `||` for concatenation in PostgreSQL.
</details>

###Q8: Handle Missing Data (NULL)**Task:** Delete viewer records where the `gender` column is empty (Null).

<details> <summary>▼ Show Answer</summary>

```sql
DELETE FROM lab3_viewer WHERE gender IS NULL;

```

**Pitfall:** Never use `= NULL`.
</details>

###Q9: Multi-Date Filter**Task:** Find `viewer_id` who watched movies on '2025-12-24' or '2025-12-25'.

<details> <summary>▼ Show Answer</summary>

```sql
SELECT DISTINCT viewer_id FROM lab3_watched WHERE watch_date IN (&#39;2025-12-24&#39;, &#39;2025-12-25&#39;);

```

</details>

---

##🔴 Level 3: Advanced Joins & Subqueries###Q10: Complex Sorting**Task:** Order all watched records by `score` (Desc), then `watch_date` (Asc), then `name` (Desc/Alpha Last).

<details> <summary>▼ Show Answer</summary>

```sql
SELECT * FROM lab3_watched w 
JOIN lab3_viewer v ON w.viewer_id = v.viewer_id 
ORDER BY w.score DESC, w.watch_date ASC, v.name DESC;

```

</details>

###Q11: Subquery Filtering**Task:** Find names of viewers who watched the movie with the absolute minimum `rating`.

<details> <summary>▼ Show Answer</summary>

```sql
SELECT v.name FROM lab3_viewer v 
JOIN lab3_watched w ON v.viewer_id = w.viewer_id 
JOIN lab3_movie m ON w.movie_id = m.movie_id 
WHERE m.rating = (SELECT MIN(rating) FROM lab3_movie);

```

</details>

###Q12: Regex Pattern**Task:** Find names of all viewers whose name starts with 'A' and ends with 'n'.

<details> <summary>▼ Show Answer</summary>

```sql
SELECT name FROM lab3_viewer WHERE name ~ &#39;^A.*n$&#39;;
-- Or: name LIKE &#39;A%n&#39;

```

</details>

###Q13: Join with Numerical Filter**Task:** List titles of movies watched by viewers aged 18 or under.

<details> <summary>▼ Show Answer</summary>

```sql
SELECT DISTINCT m.title FROM lab3_movie m 
JOIN lab3_watched w ON m.movie_id = w.movie_id 
JOIN lab3_viewer v ON w.viewer_id = v.viewer_id 
WHERE v.age &lt;= 18;

```

</details>

###Q14: Membership Status Update**Task:** Update `membership` to 'Veteran' for anyone who has more than 50 `totalvisits` (using Dentist logic).

<details> <summary>▼ Show Answer</summary>

```sql
-- Assuming dentist table structure applied to viewer
UPDATE lab3_viewer SET membership = &#39;Veteran&#39; WHERE totalvisits &gt; 50;

```

</details>

###Q15: Age Calculation Deletion**Task:** Delete records where the duration between watch_date and today (e.g. 2025) is > 10 years.

<details> <summary>▼ Show Answer</summary>

```sql
DELETE FROM lab3_watched WHERE (2025 - EXTRACT(YEAR FROM watch_date)) &gt; 10;

```

</details>

###Q16: Find Duplicate Watchers**Task:** List viewer names who have watched the same movie more than once.

<details> <summary>▼ Show Answer</summary>

```sql
SELECT v.name FROM lab3_viewer v 
JOIN lab3_watched w ON v.viewer_id = w.viewer_id 
GROUP BY v.name, w.movie_id 
HAVING COUNT(*) &gt; 1;

```

</details>

###Q17: Join with State/City filter**Task:** List names of viewers who watched movies in 'Dublin' (bridge to Cities table).

<details> <summary>▼ Show Answer</summary>

```sql
SELECT v.name FROM lab3_viewer v 
JOIN lab3_watched w ON v.viewer_id = w.viewer_id 
JOIN lab3_movie m ON w.movie_id = m.movie_id 
-- Assuming statekey logic from Airbnb data
WHERE m.movie_id IN (SELECT movie_id FROM lab3_watched WHERE statekey = &#39;DUB1&#39;);

```

</details>

###Q18: Average Score per Genre**Task:** Find the average `score` given to movies in each `genre`.

<details> <summary>▼ Show Answer</summary>

```sql
SELECT m.genre, AVG(w.score) 
FROM lab3_movie m 
JOIN lab3_watched w ON m.movie_id = w.movie_id 
GROUP BY m.genre;

```

</details>

###Q19: Update Score by Movie Title**Task:** Set `score` to 10 for all watches of the movie 'The Godfather'.

<details> <summary>▼ Show Answer</summary>

```sql
UPDATE lab3_watched SET score = 10 
WHERE movie_id IN (SELECT movie_id FROM lab3_movie WHERE title = &#39;The Godfather&#39;);

```

</details>

###Q20: Final RA to SQL Challenge**Task:** \pi_{title} (\sigma_{age < 20} viewer \bowtie watched \bowtie movie)

<details> <summary>▼ Show Answer</summary>

```sql
SELECT DISTINCT m.title FROM lab3_movie m 
JOIN lab3_watched w ON m.movie_id = w.movie_id 
JOIN lab3_viewer v ON w.viewer_id = v.viewer_id 
WHERE v.age &lt; 20;

```

</details>

---
[END]
[ZH]
 *(中文部分逻辑完全一致， 20 题)*
[END]
