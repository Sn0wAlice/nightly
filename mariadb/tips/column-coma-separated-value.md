# Column coma separated value

Pour extraire la liste de tout x,y,z, stocké dans la même colonne:  (max 5 values)

```sql
SELECT DISTINCT TRIM(SUBSTRING_INDEX(SUBSTRING_INDEX(category, ',', n), ',', -1)) AS category
FROM mytable
JOIN (
    SELECT 1 AS n UNION ALL SELECT 2 UNION ALL SELECT 3 UNION ALL SELECT 4 UNION ALL SELECT 5
) AS numbers
ON CHAR_LENGTH(category) - CHAR_LENGTH(REPLACE(category, ',', '')) >= n - 1
WHERE category IS NOT NULL;
```

et pour avoir aussi le nb d'occurrences&#x20;

```sql
SELECT category, COUNT(*) AS occurrences
FROM (
    SELECT
        TRIM(SUBSTRING_INDEX(SUBSTRING_INDEX(c.category, ',', n), ',', -1)) AS category
    FROM mytable c
    JOIN (
        SELECT 1 AS n UNION ALL SELECT 2 UNION ALL SELECT 3 UNION ALL SELECT 4 UNION ALL SELECT 5
    ) AS numbers
    ON CHAR_LENGTH(c.category) - CHAR_LENGTH(REPLACE(c.category, ',', '')) >= n - 1
    WHERE c.category IS NOT NULL
) AS extracted
GROUP BY category
ORDER BY occurrences DESC;
```
