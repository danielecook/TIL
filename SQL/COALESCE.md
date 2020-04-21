# Take the first NON-NULL value from a list of values

Given an SQL database with the table `bar`:

|   primary_key | A    | B    |   C |
|--------------:|:-----|:-----|----:|
|             1 | 50   | 100  | 200 |
|             2 | NULL | 75   |  80 |
|             3 | NULL | NULL |  20 |

You can use the SQL `COALESCE` function to retrieve a NOT NULL value among several columns. For example:

```sql
SELECT COALESCE(A,B,C) as ANY FROM bar
```
Would return:

|   ANY |
|------:|
|    50 |
|    75 |
|    20 |