# Find an element in a delimited list

You can search a comma-delimited list in SQL by using `FIND_IN_SET`. Although its considered poor design, it is possible to nest values in a field as a comma delimited list:

__fruit_table__

| fruit   | tags          |
|:--------|:--------------|
| apple   | red,delicious |
| banana  | yellow        |

You can query `tags` using `FIND_IN_SET`:

```SQL
SELECT * FROM fruit_table WHERE FIND_IN_SET('delicious', tags)
```

The above query will return

| fruit   | tags          |
|:--------|:--------------|
| apple   | red,delicious |

Normally you would want to normalize this kind of data, but it may be convenient in some instances.