### 3级标题

### q2

```sql
SELECT 
	r.name AS RELEASE_NAME, MIN(ri.date_year) AS RELEASE_YEAR
FROM 
release r
JOIN release_info ri ON r.id = ri.release
JOIN medium m ON r.id = m.release
WHERE r.artist_credit IN (SELECT id FROM artist_credit WHERE name LIKE '%The Beatles%')
AND m.format = (SELECT id FROM medium_format WHERE name = '12" Vinyl')
AND ri.area = (SELECT id FROM area WHERE name = 'United Kingdom')
AND ri.date_year <= (SELECT MAX(end_date_year) FROM artist WHERE name LIKE '%The Beatles%')
GROUP BY r.name
```

