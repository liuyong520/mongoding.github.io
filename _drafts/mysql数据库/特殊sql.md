复杂sql
===




## 复杂SQL编写
### 查出每门课都大于80分的学生

```sql
SELECT name
FROM   table
GROUP  BY name
HAVING MIN(fenshu) > 80
```

### 删除重复数据

```sql
DELETE FROM tablename
WHERE  id NOT IN(SELECT MIN(id)
                 FROM   tablename
                 GROUP  BY student_no,
                           student_name,
                           course_id,
                           course_name,
                           score)
```

### 数据转置

```sql
SELECT year,
       (SELECT amount
        FROM   sale m
        WHERE  month = 1
               AND m.year = sale.year) AS m1,
       (SELECT amount
        FROM   sale m
        WHERE  month = 2
               AND m.year = sale.year) AS m2,
       (SELECT amount
        FROM   sale m
        WHERE  month = 3
               AND m.year = sale.year) AS m3,
       (SELECT amount
        FROM   sale m
        WHERE  month = 4
               AND m.year = sale.year) AS m4
FROM   sale
GROUP  BY year
```

### 批量更新

```sql
UPDATE T_DEMO_ZYCL_WFXW
SET    WFLX = CASE ID
                WHEN '1' THEN '4'
                WHEN '2' THEN '3'
                WHEN '3' THEN '1'
                WHEN '4' THEN '2'
                WHEN '5' THEN '5'
              END
WHERE ID IN ('1','2','3','4','5')
```

### 查询第10条到第20条数据

```sql
SELECT * FROM sale LIMIT 10, 20
```

### 查询每个用户最新的订单数据

```sql
SELECT
	*
FROM
	ORDER o1
WHERE
	o1.create_time = (
		SELECT
			MAX(o2.create_time)
		FROM
			ORDER o2
		WHERE
			o2.user_id = o1.user_id
		GROUP BY
			o2.user_id
	)
```
