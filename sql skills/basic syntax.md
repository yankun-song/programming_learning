# Insert

# Delete
```
DELETE FROM courses WHERE name = 'Dynamic Programming'
```

# Update

# Select

## group by
```
select bike_id, user_id from shared_bicycles
group by bike_id, user_id
having count(1) >= 3
```
## bewteen
Between is inclusive
```
SELECT name from courses 
-- where name between 'Db' and 'Dz'
WHERE name BETWEEN 'Db' AND 'Dz'
```

## distinct
```
SELECT DISTINCT age FROM teachers 
ORDER BY age 
```
## null
### isnull
> The ISNULL() function returns 1 or 0 depending on whether an expression is NULL.

`ISNULL(expression)`

### ifnull
`IFNULL(expression, alt_value)`

### coalesce
> The COALESCE() function returns the first non-null value in a list

`COALESCE(val1, val2, ...., val_n)`

so COALESCE(expression, 0) can realize the same result as above.

## extract year/ month from datetime
```SELECT name, year(created_at) AS year, month(created_at) AS month FROM courses```

##
