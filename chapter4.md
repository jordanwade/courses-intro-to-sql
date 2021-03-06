---
title       : Sorting, grouping and joins
description : This chapter provides a brief introduction to sorting and grouping your results, and briefly touches on the concept of joins.

---
## ORDER BY

```yaml
type: PureMultipleChoiceExercise
lang: sql
xp: 50
key: 322af4938b
```

Congratulations on making it this far! You now know how to select and filter your results.

In this chapter you'll learn how to sort and group your results to gain further insight. Let's go!

In SQL, the `ORDER BY` keyword is used to sort results in ascending or descending order according to the values of one or more columns. 

By default `ORDER BY` will sort in ascending order. If you want to sort the results in descending order, you can use the `DESC` keyword. For example,

```
SELECT title
FROM films
ORDER BY release_year DESC;
```

gives you the titles of films sorted by release year, from newest to oldest.

<hr>
How do you think `ORDER BY` sorts a column of text values by default?

`@possible_answers`
- [Alphabetically (A-Z)]
- Reverse alphabetically (Z-A)
- There's no natural ordering to text data
- By number of characters (fewest to most)

`@hint`
By default, `ORDER BY` sorts alphabetically, but in which direction?

`@feedback`

- Correct!
- Incorrect. Although text values are ordered alphabetically, they don't go Z-A.
- Incorrect. Text values are ordered alphabetically.
- Incorrect. Text values are ordered alphabetically.


---
## Sorting single columns

```yaml
type: BulletExercise
lang: sql
xp: 100
key: a7b2964ba6
```

Now that you understand how `ORDER BY` works, give these exercises a go!

`@pre_exercise_code`
```{python}
connect('postgresql', 'films')
set_options(visible_tables = ['films', 'people'])
```

`@sample_code`
```{sql}

```

***

```yaml
type: NormalExercise
key: e3a06cce15
xp: 30
```

`@instructions`
Get the names of people from the `people` table, sorted alphabetically.
`@solution`
```{sql}
SELECT name
FROM people
ORDER BY name;
```

`@hint`
```
SELECT ___
FROM ___
ORDER BY ___;
```

`@sct`
```{python}
sel = check_node('SelectStmt')

name = test_column('name', msg='Did you select the `name` column correctly?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

order_by_clause = sel.check_field('order_by_clause').has_equal_ast('Is your `ORDER BY` clause correct?')

Ex().test_correct(check_result(), [
    order_by_clause,
    from_clause,
    name,
    test_has_columns(),
    test_ncols(),
    test_error()
])

```

***

```yaml
type: NormalExercise
key: 14a2792baa
xp: 30
```

`@instructions`
Get the names of people, sorted by birth date.
`@solution`
```{sql}
SELECT name
FROM people
ORDER BY birthdate;
```
`@hint`
```
SELECT ___
FROM ___
ORDER BY ___;
```
`@sct`
```{sql}
sel = check_node('SelectStmt')

name = test_column('name', msg='Did you select the `name` column correctly?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

order_by_clause = sel.check_field('order_by_clause').has_equal_ast('Is your `ORDER BY` clause correct?')

Ex().test_correct(check_result(), [
    order_by_clause,
    from_clause,
    name,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

***

```yaml
type: NormalExercise
key: 6cfeca71b1
xp: 30
```

`@instructions`
Get the birth date and name for every person, in order of when they were born.
`@solution`
```{sql}
SELECT birthdate, name
FROM people
ORDER BY birthdate;
```
`@hint`
```
SELECT ___, ___
FROM ___
ORDER BY ___;
```
`@sct`
```{python}
sel = check_node('SelectStmt')

birthdate = test_column('birthdate', msg='Did you select the `birthdate` column correctly?')

name = test_column('name', msg='Did you select the `name` column correctly?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

order_by_clause = sel.check_field('order_by_clause').has_equal_ast('Is your `ORDER BY` clause correct?')

Ex().test_correct(check_result(), [
    order_by_clause,
    from_clause,
    birthdate,
    name,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

---
## Sorting single columns (2)

```yaml
type: BulletExercise
lang: sql
xp: 100
key: 357ec9bc3d
```

Let's get some more practice with `ORDER BY`!

`@pre_exercise_code`
```{python}
connect('postgresql', 'films')
set_options(visible_tables = ['films', 'people'])
```

`@sample_code`
```{sql}

```

***

```yaml
type: NormalExercise
key: e2702b5933
xp: 30
```

`@instructions`
Get the title of films released in 2000 or 2012, in the order they were released.
`@solution`
```{sql}
SELECT title
FROM films
WHERE release_year IN (2000, 2012)
ORDER BY release_year;
```
`@hint`
```
SELECT ___
FROM ___
WHERE ___ IN (___, ___)
ORDER BY ___;
```
`@sct`
```{python}
sel = check_node('SelectStmt')

title = test_column('title', msg='Did you select the `title` column correctly?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

order_by_clause = sel.check_field('order_by_clause').has_equal_ast('Is your `ORDER BY` clause correct?')

where_clause = sel.check_field('where_clause')

in_thing = where_clause.has_equal_ast(sql="release_year IN (2000, 2012)", start='expression', exact=False, msg='Did you use `IN` correctly?')

Ex().test_correct(check_result(), [
    order_by_clause,
    from_clause,
    in_thing,
    title,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

***

```yaml
type: NormalExercise
key: 5c84507976
xp: 30
```

`@instructions`
Get all details for all films except those released in 2015 and order them by duration.
`@solution`
```{sql}
SELECT *
FROM films
WHERE release_year <> 2015
ORDER BY duration;
```
`@hint`
```{sql}
SELECT ___
FROM ___
WHERE ___ <> ___
ORDER BY ___;
```
`@sct`
```{python}
sel = check_node('SelectStmt')

star = sel.check_node('Star').has_equal_ast('Are you selecting all columns correctly?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause')

where_release_year = where_clause.has_equal_ast(sql="release_year <> 2015", start='expression', exact=False, msg='Did you use check the `release_year` with `<>` correctly in your `WHERE` clause?')

order_by_clause = sel.check_field('order_by_clause').has_equal_ast('Is your `ORDER BY` clause correct?')

Ex().test_correct(check_result(), [
    from_clause,
    order_by_clause,
    where_release_year,
    star,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

***

```yaml
type: NormalExercise
key: 6359addbf8
xp: 30
```

`@instructions`
Get the title and gross earnings for movies which begin with the letter 'M' and order the results alphabetically.
`@solution`
```{sql}
SELECT title, gross
FROM films
WHERE title LIKE 'M%'
ORDER BY title;
```
`@hint`
```{sql}
SELECT ___, ___
FROM ___
WHERE ___ LIKE ___
ORDER BY ___;
```
`@sct`
```{python}
sel = check_node('SelectStmt')

title = test_column('title', msg='Did you select the `title` column correctly?')

gross = test_column('gross', msg='Did you select the `gross` column correctly?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause')

left_like = where_clause.check_field('left').has_equal_ast('Are you using `title` with `LIKE`?')
op_like = where_clause.check_field('op').has_equal_ast('Are you using the `LIKE` operator in your `WHERE` clause?')
right_like = where_clause.check_field('right').has_equal_ast("Are you using `LIKE` with `'M%'`?")

order_by_clause = sel.check_field('order_by_clause').has_equal_ast('Is your `ORDER BY` clause correct?')

Ex().test_correct(check_result(), [
    from_clause,
    order_by_clause,
    left_like,
    op_like,
    right_like,
    title,
    gross,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

---
## Sorting single columns (DESC)

```yaml
type: BulletExercise
lang: sql
xp: 100
key: a7b2964ba7
```

To order results in _descending_ order, you can put the keyword `DESC` after your `ORDER BY`. For example, to get all the names in the `people` table, in reverse alphabetical order:

```
SELECT name
FROM people
ORDER BY name DESC;
```

Now practice using `ORDER BY` with `DESC` to sort single columns in descending order!

`@pre_exercise_code`
```{python}
connect('postgresql', 'films')
set_options(visible_tables = ['films', 'reviews'])
```

`@sample_code`
```{sql}

```

***

```yaml
type: NormalExercise
key: 1ed651456d
xp: 30
```

`@instructions`
Get the IMDB score and film ID for every film from the reviews table, sorted from highest to lowest score.

`@solution`
```{sql}
SELECT imdb_score, film_id
FROM reviews
ORDER BY imdb_score DESC;
```

`@hint`
```
SELECT ___, ___
FROM ___
ORDER BY ___ DESC;
```

`@sct`
```{python}
sel = check_node('SelectStmt')

imdb_score = test_column('imdb_score', msg='Did you select the `imdb_score` column correctly?')
film_id = test_column('film_id', msg='Did you select the `film_id` column correctly?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

order_by_clause = sel.check_field('order_by_clause').has_equal_ast('Is your `ORDER BY` clause correct?')

Ex().test_correct(check_result(), [
    order_by_clause,
    from_clause,
    imdb_score,
    film_id,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

***

```yaml
type: NormalExercise
key: 3629dd5dcd
xp: 30
```

`@instructions`
Get the title for every film, in reverse order.

`@solution`
```{sql}
SELECT title
FROM films
ORDER BY title DESC;
```
`@hint`
```
SELECT ___
FROM ___
ORDER BY ___ ___;
```
`@sct`
```{python}
sel = check_node('SelectStmt')

title = test_column('title', msg='Did you select the `title` column correctly?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

order_by_clause = sel.check_field('order_by_clause').has_equal_ast('Is your `ORDER BY` clause correct?')

Ex().test_correct(check_result(), [
    order_by_clause,
    from_clause,
    title,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

***

```yaml
type: NormalExercise
key: ddcb2dd3ad
xp: 30
```

`@instructions`
Get the title and duration for every film, in order of longest duration to shortest.

`@solution`
```{sql}
SELECT title, duration
FROM films
ORDER BY duration DESC;
```
`@hint`
```
SELECT ___, ___
FROM ___
ORDER BY ___ ___;
```
`@sct`
```{python}
sel = check_node('SelectStmt')

title = test_column('title', msg='Did you select the `title` column correctly?')

duration = test_column('duration', msg='Did you select the `duration` column correctly?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

order_by_clause = sel.check_field('order_by_clause').has_equal_ast('Is your `ORDER BY` clause correct?')

Ex().test_correct(check_result(), [
    order_by_clause,
    from_clause,
    title,
    duration,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

---
## Sorting multiple columns

```yaml
type: BulletExercise
lang: sql
xp: 100
key: b2a52993bc
```

`ORDER BY` can also be used to sort on multiple columns. It will sort by the first column specified, then sort by the next, then the next, and so on. For example,

```
SELECT birthdate, name
FROM people
ORDER BY birthdate, name;
```

sorts on birth dates first (oldest to newest) and then sorts on the names in alphabetical order. **The order of columns is important!**

Try using `ORDER BY` to sort multiple columns! Remember, to specify multiple columns you separate the column names with a comma.

`@pre_exercise_code`
```{python}
connect('postgresql', 'films')
set_options(visible_tables = ['films', 'people'])
```

`@sample_code`
```{sql}

```

***

```yaml
type: NormalExercise
key: 9ec6e8ae72
xp: 30
```

`@instructions`
Get the birth date and name of people in the `people` table, in order of when they were born and alphabetically by name.
`@solution`
```{sql}
SELECT birthdate, name
FROM people
ORDER BY birthdate, name;
```
`@hint`
```
SELECT ___, ___
FROM ___
ORDER BY ___, ___;
```

`@sct`
```{python}
sel = check_node('SelectStmt')

birthdate = test_column('birthdate', msg='Did you select the `birthdate` column correctly?')

name = test_column('name', msg='Did you select the `name` column correctly?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

order_by_clause = sel.check_field('order_by_clause').has_equal_ast('Is your `ORDER BY` clause correct?')

Ex().test_correct(check_result(), [
    order_by_clause,
    from_clause,
    birthdate,
    name,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

***

```yaml
type: NormalExercise
key: 3460b2f14b
xp: 30
```

`@instructions`
Get the release year, duration, and title of films ordered by their release year and duration.
`@solution`
```{sql}
SELECT release_year, duration, title
FROM films
ORDER BY release_year, duration;
```
`@hint`
```
SELECT ___, ___, ___
FROM ___
ORDER BY ___, ___;
```
`@sct`
```{python}
sel = check_node('SelectStmt')

release_year = test_column('release_year', msg='Did you select the `release_year` column correctly?')

duration = test_column('duration', msg='Did you select the `duration` column correctly?')

title = test_column('title', msg='Did you select the `title` column correctly?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

order_by_clause = sel.check_field('order_by_clause').has_equal_ast('Is your `ORDER BY` clause corect?')

Ex().test_correct(check_result(), [
    order_by_clause,
    from_clause,
    release_year,
    duration,
    title,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

***

```yaml
type: NormalExercise
key: c03517c2b9
xp: 30
```

`@instructions`
Get certifications, release years, and titles of films ordered by certification (alphabetically) and release year.

`@solution`
```{sql}
SELECT certification, release_year, title
FROM films
ORDER BY certification, release_year;
```
`@hint`
```
SELECT ___, ___, ___
FROM ___
ORDER BY ___, ___;
```
`@sct`
```{python}
sel = check_node('SelectStmt')

certification = test_column('certification', msg='Did you select the `certification` column correctly?')

release_year = test_column('release_year', msg='Did you select the `release_year` column correctly?')

title = test_column('title', msg='Did you select the `title` column correctly?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

order_by_clause = sel.check_field('order_by_clause').has_equal_ast('Is your `ORDER BY` clause corect?')

where_clause = sel.check_field('where_clause').has_equal_ast('Is your `WHERE` clause correct?')

Ex().test_correct(check_result(), [
    order_by_clause,
    from_clause,
    certification,
    release_year,
    title,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

***

```yaml
type: NormalExercise
key: f0ade213ff
xp: 30
```

`@instructions`
Get the names and birthdates of people ordered by name and birth date.
`@solution`
```{sql}
SELECT name, birthdate
FROM people
ORDER BY name, birthdate;
```
`@hint`
```
SELECT ___, ___
FROM ___
ORDER BY ___, ___;
```
`@sct`
```{python}
sel = check_node('SelectStmt')

name = test_column('name', msg='Did you select the `name` column correctly?')

birthdate = test_column('birthdate', msg='Did you select the `birthdate` column correctly?')

from_clause = sel.check_field('from_clause')

where_clause = sel.check_field('where_clause').has_equal_ast('Is your `WHERE` clause correct?')

order_by_clause = sel.check_field('order_by_clause').has_equal_ast('Is your `ORDER BY` clause correct?')

Ex().test_correct(check_result(), [
    order_by_clause,
    from_clause,
    name,
    birthdate,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

---
## GROUP BY

```yaml
type: PureMultipleChoiceExercise
lang: sql
xp: 50
key: 81987a99cf
```

Now you know how to sort results! Often you'll need to aggregate results. For example, you might want to get count the number of male and female employees in your company. Here, what you want is to group all the males together and count them, and group all the females together and count them. In SQL, `GROUP BY` allows you to group a result by one or more columns, like so:

```
SELECT sex, count(*)
FROM employees
GROUP BY sex;
```

This might give, for example:

| sex | count |
|----|----|
| male | 15 |
| female | 19 |

<br>
Commonly, `GROUP BY` is used with _aggregate functions_ like `COUNT()` or `MAX()`. Note that `GROUP BY` always goes after the `FROM` clause!

<hr>
What is `GROUP BY` used for?

`@possible_answers`
- Performing operations by column
- Performing operations all at once
- Performing operations in a particular order
- [Performing operations by group]

`@hint`
You use `GROUP BY` when you want to compute results within groups.

`@feedback`
- Incorrect. While `GROUP BY` does sort by column, we could just use `ORDER BY` for this.
- Incorrect.
- Incorrect. While `GROUP BY` does sort results, it's not designed to control order of operations.
- Correct! `GROUP BY` is for performing operations within groups.


---
## GROUP BY practice

```yaml
type: BulletExercise
lang: sql
xp: 100
key: 98e30a6131
```

As you've just seen, combining aggregate functions with `GROUP BY` can yield some powerful results!

A word of warning: SQL will return an error if you try to `SELECT` a field that is not in your `GROUP BY` clause without using it to calculate some kind of value about the entire group.

Note that you can combine `GROUP BY` with `ORDER BY` to group your results, calculate something about them, and then order your results. For example,

```
SELECT sex, count(*)
FROM employees
GROUP BY sex
ORDER BY count DESC;
```

might return something like

| sex | count |
|----|----|
| female | 19 |
| male | 15 |

<br>
because there are more females at our company than males. Note also that `ORDER BY` always goes after `GROUP BY`. Let's try some exercises!

`@pre_exercise_code`
```{python}
connect('postgresql', 'films')
set_options(visible_tables = ['films', 'reviews'])
```

`@sample_code`
```{sql}

```

***

```yaml
type: NormalExercise
key: 0b29eb4ff3
xp: 30
```

`@instructions`
Get the release year and count of films released in each year.
`@solution`
```{sql}
SELECT release_year, COUNT(*)
FROM films
GROUP BY release_year;
```
`@hint`
```
SELECT ___, ___(___)
FROM ___
GROUP BY ___;
```
`@sct`
```{python}
sel = check_node('SelectStmt')

release_year = test_column('release_year', msg='Did you select the `release_year` column correctly?')

temp = sel.check_node('Call')
count_call = temp.check_field('name').has_equal_ast('Did you use the `COUNT` function?')
count_args = temp.check_field('args').has_equal_ast('Are you using `COUNT` on the right column?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

group_by_clause = sel.check_field('group_by_clause').has_equal_ast('Is your `GROUP BY` clause correct?')

Ex().test_correct(check_result(), [
    group_by_clause,
    from_clause,
    release_year,
    count_call,
    count_args,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

***

```yaml
type: NormalExercise
key: ebee043890
xp: 30
```

`@instructions`
Get the release year and average duration of all films, grouped by release year.
`@solution`
```{sql}
SELECT release_year, AVG(duration)
FROM films
GROUP BY release_year;
```
`@hint`
```
SELECT ___, ___(___)
FROM ___
GROUP BY ___;
```
`@sct`
```{python}
sel = check_node('SelectStmt')

release_year = test_column('release_year', msg='Did you select the `release_year` column correctly?')

temp = sel.check_node('Call')
avg_call = temp.check_field('name').has_equal_ast('Did you use the `AVG` function?')
avg_args = temp.check_field('args').has_equal_ast('Are you using `AVG` on the right column?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

group_by_clause = sel.check_field('group_by_clause').has_equal_ast('Is your `GROUP BY` clause correct?')

Ex().test_correct(check_result(), [
    group_by_clause,
    from_clause,
    release_year,
    avg_call,
    avg_args,
    test_has_columns(),
    test_ncols(),
    test_error()
])

```

***

```yaml
type: NormalExercise
key: b4341b8451
xp: 30
```

`@instructions`
Get the release year and largest budget for all films, grouped by release year.
`@solution`
```{sql}
SELECT release_year, MAX(budget)
FROM films
GROUP BY release_year;
```
`@hint`
```
SELECT ___, ___(___)
FROM ___
GROUP BY ___;
```
`@sct`
```{python}
sel = check_node('SelectStmt')

release_year = test_column('release_year', msg='Did you select the `release_year` column correctly?')

temp = sel.check_node('Call')
max_call = temp.check_field('name').has_equal_ast('Did you use the `MAX` function?')
max_args = temp.check_field('args').has_equal_ast('Are you using `MAX` on the right column?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

group_by_clause = sel.check_field('group_by_clause').has_equal_ast('Is your `GROUP BY` clause correct?')

Ex().test_correct(check_result(), [
    group_by_clause,
    from_clause,
    release_year,
    max_call,
    max_args,
    test_has_columns(),
    test_ncols(),
    test_error()
])

```

***

```yaml
type: NormalExercise
key: 66d0c5198a
xp: 30
```

`@instructions`
Get the IMDB score and count of film reviews grouped by IMDB score in the `reviews` table.
`@solution`
```{sql}
SELECT imdb_score, COUNT(*)
FROM reviews
GROUP BY imdb_score;
```

`@hint`
```
SELECT ___, ___(___)
FROM ___
GROUP BY ___;
```
`@sct`
```{python}
sel = check_node('SelectStmt')

imdb_score = test_column('imdb_score', msg='Did you select the `imdb_score` column correctly?')

temp = sel.check_node('Call')

count_call = temp.check_field('name').has_equal_ast('Are you calling the `COUNT` function?')

count_args = temp.check_field('args').has_equal_ast('Are you using `COUNT` on the right column?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

group_by_clause = sel.check_field('group_by_clause').has_equal_ast('Is your `GROUP BY` clause correct?')

Ex().test_correct(check_result(), [
    group_by_clause,
    from_clause,
    imdb_score,
    count_call,
    count_args,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

---
## GROUP BY practice (2)

```yaml
type: BulletExercise
lang: sql
xp: 100
key: 38a7c62434
```

Now practice your new skills by combining `GROUP BY` and `ORDER BY` with some more aggregate functions!

Make sure to always put the `ORDER BY` clause at the end of your query. You can't sort values that you haven't calculated yet!

`@pre_exercise_code`
```{python}
connect('postgresql', 'films')
set_options(visible_tables = ['films'])
```

`@sample_code`
```{sql}

```

***

```yaml
type: NormalExercise
key: 53ad6da98c
xp: 30
```

`@instructions`
Get the release year and lowest gross earnings per release year.
`@solution`
```{sql}
SELECT release_year, MIN(gross)
FROM films
GROUP BY release_year;
```
`@hint`
```
SELECT ___, ___(___)
FROM ___
GROUP BY ___;
```
`@sct`
```{python}
sel = check_node('SelectStmt')

release_year = test_column('release_year', msg='Did you select the `release_year` column correctly?')

temp = sel.check_node('Call')
min_call = temp.check_field('name').has_equal_ast('Did you use the `MIN` function?')
min_args = temp.check_field('args').has_equal_ast('Are you using `MIN` on the right column?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

group_by_clause = sel.check_field('group_by_clause').has_equal_ast('Is your `GROUP BY` clause correct?')

Ex().test_correct(check_result(), [
    group_by_clause,
    from_clause,
    release_year,
    min_call,
    min_args,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

***

```yaml
type: NormalExercise
key: cff5924de5
xp: 30
```

`@instructions`
Get the language and total gross amount films in each language made.
`@solution`
```{sql}
SELECT language, SUM(gross)
FROM films
GROUP BY language;
```
`@hint`
```
SELECT ___, ___(___)
FROM ___
GROUP BY ___;
```
`@sct`
```{python}
sel = check_node('SelectStmt')

language = test_column('language', msg='Did you select the `language` column correctly?')

temp = sel.check_node('Call')
sum_call = temp.check_field('name').has_equal_ast('Did you use the `SUM` function?')
sum_args = temp.check_field('args').has_equal_ast('Are you using `SUM` on the right column?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

group_by_clause = sel.check_field('group_by_clause').has_equal_ast('Is your `GROUP BY` clause correct?')

Ex().test_correct(check_result(), [
    group_by_clause,
    from_clause,
    language,
    sum_call,
    sum_args,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

***

```yaml
type: NormalExercise
key: 83944ff64f
xp: 30
```

`@instructions`
Get the country and total budget spent making movies in each country.
`@solution`
```{sql}
SELECT country, SUM(budget)
FROM films
GROUP BY country;
```
`@hint`
```
SELECT ___, ___(___)
FROM ___
GROUP BY ___;
```
`@sct`
```{python}
sel = check_node('SelectStmt')

country = test_column('country', msg='Did you select the `country` column correctly?')

temp = sel.check_node('Call')
sum_call = temp.check_field('name').has_equal_ast('Did you use the `SUM` function?')
sum_args = temp.check_field('args').has_equal_ast('Are you using `SUM` on the right column?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

group_by_clause = sel.check_field('group_by_clause').has_equal_ast('Is your `GROUP BY` clause correct?')

Ex().test_correct(check_result(), [
    group_by_clause,
    from_clause,
    sum_call,
    country,
    sum_args,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

***

```yaml
type: NormalExercise
key: eac5f722a5
xp: 30
```

`@instructions`
Get the release year, country, and highest budget spent making a film for each year, for each country. Sort your results by release year and country.
`@solution`
```{sql}
SELECT release_year, country, MAX(budget)
FROM films
GROUP BY release_year, country
ORDER BY release_year, country;
```
`@hint`
```
SELECT ___, ___, ___(___)
FROM ___
GROUP BY ___, ___
ORDER BY ___, ___;
```
`@sct`
```{python}
sel = check_node('SelectStmt')

release_year = test_column('release_year', msg='Did you select the `release_year` column correctly?')

country = test_column('country', msg='Did you select the `country` column correctly?')

temp = sel.check_node('Call')
max_call = temp.check_field('name').has_equal_ast('Did you use the `MAX` function?')
max_args = temp.check_field('args').has_equal_ast('Are you using `MAX` on the right column?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

order_by_clause = sel.check_field('order_by_clause').has_equal_ast('Is your `ORDER BY` clause correct?')

group_by_clause = sel.check_field('group_by_clause').has_equal_ast('Is your `GROUP BY` clause correct?')

Ex().test_correct(check_result(), [
    order_by_clause,
    group_by_clause,
    from_clause,
    max_call,
    max_args,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

***

```yaml
type: NormalExercise
key: 48461bd4d3
xp: 30
```

`@instructions`
Get the country, release year, and lowest amount grossed per release year per country. Order your results by country and release year.
`@solution`
```{sql}
SELECT country, release_year, MIN(gross)
FROM films
GROUP BY country, release_year
ORDER BY country, release_year;
```
`@hint`
```
SELECT ___, ___, ___(___)
FROM films
GROUP BY country, release_year
ORDER BY ___, ___;
```
`@sct`
```{python}
sel = check_node('SelectStmt')

release_year = test_column('release_year', msg='Did you select the `release_year` column correctly?')

country = test_column('country', msg='Did you select the `country` column correctly?')

temp = sel.check_node('Call')
min_call = temp.check_field('name').has_equal_ast('Did you use the `MIN` function?')
min_args = temp.check_field('args').has_equal_ast('Are you using `MIN` on the right column?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

order_by_clause = sel.check_field('order_by_clause').has_equal_ast('Is your `ORDER BY` clause correct?')

group_by_clause = sel.check_field('group_by_clause').has_equal_ast('Is your `GROUP BY` clause correct?')

Ex().test_correct(check_result(), [
    order_by_clause,
    group_by_clause,
    from_clause,
    country,
    release_year,
    min_call,
    min_args,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

---
## HAVING a great time

```yaml
type: MultipleChoiceExercise
lang: sql
xp: 50
key: a391d35885
```

In SQL, aggregate functions can't be used in `WHERE` clauses. For example, the following query is invalid:

```
SELECT release_year
FROM films
GROUP BY release_year
WHERE COUNT(title) > 10;
```

This means that if you want to filter based on the result of an aggregate function, you need another way! That's where the `HAVING` clause comes in. For example,

```
SELECT release_year
FROM films
GROUP BY release_year
HAVING COUNT(title) > 10;
```

shows only those years in which more than 10 films were released.

<hr>
In how many different years were more than 200 movies released?

`@instructions`
- 12
- 13
- 14
- 15

`@hint`
Replace 10 with 200 in the query above and run it in the editor.

`@pre_exercise_code`
```{python}
connect('postgresql', 'films')
set_options(visible_tables = ['films'])
```

`@sample_code`
```{sql}
-- You can test out queries here!

```

`@sct`
```{python}
msg1 = 'Correct!'
msg2 = 'Incorrect! Make a small modification to the query above and run it in the editor.'

Ex().test_mc(2, [msg2, msg1, msg2, msg2])
```

---
## All together now

```yaml
type: TabExercise
lang: sql
xp: 100
key: f7dcb9e122
```

Time to practice using `ORDER BY`, `GROUP BY` and `HAVING` together.

Now you're going to write a query that returns the average budget and average gross earnings for films in each year after 1990, if the average budget is greater than $60 million.

This is going to be a big query, but you can handle it!


`@pre_exercise_code`
```{python}
connect('postgresql', 'films')
set_options(visible_tables = ['films'])
```

`@sample_code`
```{sql}

```

***

```yaml
type: NormalExercise
key: a141cd669f
xp: 30
```

`@instructions`
Get the release year, budget and gross earnings for each film in the `films` table.
`@solution`
```{sql}
SELECT release_year, budget, gross
FROM films;
```
`@hint`
```
SELECT ___, ___, ___
FROM ___;
```
`@sct`
```{python}
sel = check_node('SelectStmt')

release_year = test_column('release_year', msg='Did you select the `release_year` column correctly?')

budget = test_column('budget', msg='Did you select the `budget` column correctly?')

gross = test_column('gross', msg='Did you select the `release_year` column correctly?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

Ex().test_correct(check_result(), [
    from_clause,
    release_year,
    budget,
    gross,
    test_has_columns(),
    test_ncols(),
    test_error()
])

```

***

```yaml
type: NormalExercise
key: d80bd57b13
xp: 30
```

`@instructions`
Modify your query so that only results after 1990 are included.
`@solution`
```{sql}
SELECT release_year, budget, gross
FROM films
WHERE release_year > 1990;
```
`@hint`
```
SELECT ___, ___, ___
FROM ___
WHERE ___ > ___;
```
`@sct`
```{python}
sel = check_node('SelectStmt')

release_year = test_column('release_year', msg='Did you select the `release_year` column correctly?')

budget = test_column('budget', msg='Did you select the `budget` column correctly?')

gross = test_column('gross', msg='Did you select the `gross` column correctly?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause')

where_release_year = where_clause.has_equal_ast(sql='release_year > 1990', start='expression', exact=False, msg='Did you check the `release_year` correctly in your `WHERE` clause?')

Ex().test_correct(check_result(), [
    from_clause,
    where_release_year,
    release_year,
    budget,
    gross,
    test_has_columns(),
    test_ncols(),
    test_error()
])

```

***

```yaml
type: NormalExercise
key: 2d5e6bd8cf
xp: 30
```

`@instructions`
Remove the budget and gross columns, and group your results by release year.
`@solution`
```{sql}
SELECT release_year
FROM films
WHERE release_year > 1990
GROUP BY release_year;
```
`@hint`
```
SELECT ___
FROM ___
WHERE ___ > ___
GROUP BY ___;
```
`@sct`
```{python}
sel = check_node('SelectStmt')

release_year = test_column('release_year', msg='Did you select the `release_year` column correctly?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

group_by_clause = sel.check_field('group_by_clause').has_equal_ast('Is your `GROUP BY` clause correct?')

where_clause = sel.check_field('where_clause')

where_release_year = where_clause.has_equal_ast(sql='release_year > 1990', start='expression', exact=False, msg='Did you check the `release_year` correctly in your `WHERE` clause?')

Ex().test_correct(check_result(), [
    group_by_clause,
    from_clause,
    where_clause,
    release_year,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

***

```yaml
type: NormalExercise
key: ee92d8cbaa
xp: 30
```

`@instructions`
Modify your query to add in the average budget and average gross earnings for the results you have so far. Alias your results as `avg_budget` and `avg_gross`, respectively.
`@solution`
```{sql}
SELECT release_year, AVG(budget) AS avg_budget, AVG(gross) AS avg_gross
FROM films
WHERE release_year > 1990
GROUP BY release_year;
```
`@hint`
```
SELECT ___, ___(___) AS avg_budget, AVG(gross) AS avg_gross
FROM ___
WHERE ___ > ___
GROUP BY ___;
```
`@sct`
```{python}
sel = check_node('SelectStmt')

release_year = test_column('release_year', msg='Did you select the `release_year` column correctly?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause')
where_release_year = where_clause.has_equal_ast(sql='release_year > 1990', start='expression', exact=False, msg='Did you check the `release_year` correctly in your `WHERE` clause?')

group_by_clause = sel.check_field('group_by_clause').has_equal_ast('Is your `GROUP BY` clause correct?')

alias1 = test_column('avg_budget', match='exact', msg='Are you aliasing `avg_budget` correctly?')
alias2 = test_column('avg_gross', match='exact', msg='Are you aliasing `avg_gross` correctly?')

Ex().test_correct(check_result(), [
    group_by_clause,
    where_clause,
    from_clause,
    where_release_year,
    alias1,
    alias2,
    release_year,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

***

```yaml
type: NormalExercise
key: ce0f248734
xp: 30
```

`@instructions`
Modify your query so that only years with an average budget of greater than $60 million are included.

`@solution`
```{sql}
SELECT release_year, AVG(budget) AS avg_budget, AVG(gross) AS avg_gross
FROM films
WHERE release_year > 1990
GROUP BY release_year
HAVING AVG(budget) > 60000000;
```
`@hint`
```
SELECT ___, AVG(budget) AS avg_budget, AVG(gross) AS avg_gross
FROM ___
WHERE ___ > ___
GROUP BY ___
HAVING AVG(___) > 60000000;
```
`@sct`
```{python}
sel = check_node('SelectStmt')

release_year = test_column('release_year', msg='Did you select the `release_year` column correctly?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause')
where_release_year = where_clause.has_equal_ast(sql='release_year > 1990', start='expression', exact=False, msg='Did you check the `release_year` correctly in your `WHERE` clause?')

group_by_clause = sel.check_field('group_by_clause').has_equal_ast('Is your `GROUP BY` clause correct?')

alias1 = test_column('avg_budget', match='exact', msg='Are you aliasing `avg_budget` correctly?')
alias2 = test_column('avg_gross', match='exact', msg='Are you aliasing `avg_gross` correctly?')

having_clause = sel.check_field('having_clause').has_equal_ast('Is your `HAVING` clause correct?')

avg_in_having = having_clause.check_node('Call').has_equal_ast('Are you correctly calling `AVG` on `budget` in your `HAVING` clause?')

Ex().test_correct(check_result(), [
    group_by_clause,
    avg_in_having,
    having_clause,
    from_clause,
    where_release_year,
    alias1,
    alias2,
    release_year,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

***

```yaml
type: NormalExercise
key: 7b3afeed2f
xp: 30
```

`@instructions`
Finally, modify your query to order the results from highest average gross earnings to lowest.

`@solution`
```{sql}
SELECT release_year, AVG(budget) AS avg_budget, AVG(gross) AS avg_gross
FROM films
WHERE release_year > 1990
GROUP BY release_year
HAVING AVG(budget) > 60000000
ORDER BY avg_gross DESC;
```
`@hint`
```
SELECT ___, AVG(budget) AS avg_budget, AVG(gross) AS avg_gross
FROM ___
WHERE ___ > ___
GROUP BY ___
HAVING AVG(___) > 60000000
ORDER BY ___ DESC;
```
`@sct`
```{python}
sel = check_node('SelectStmt')

release_year = test_column('release_year', msg='Did you select the `release_year` column correctly?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause')
where_release_year = where_clause.has_equal_ast(sql='release_year > 1990', start='expression', exact=False, msg='Did you check the `release_year` correctly in your `WHERE` clause?')

group_by_clause = sel.check_field('group_by_clause').has_equal_ast('Is your `GROUP BY` clause correct?')

alias1 = test_column('avg_budget', match='exact', msg='Are you aliasing `avg_budget` correctly?')
alias2 = test_column('avg_gross', match='exact', msg='Are you aliasing `avg_gross` correctly?')

having_clause = sel.check_field('having_clause').has_equal_ast('Is your `HAVING` clause correct?')

avg_in_having = having_clause.check_node('Call').has_equal_ast('Are you correctly calling `AVG` on `budget` in your `HAVING` clause?')

order_by_clause = sel.check_field('order_by_clause').has_equal_ast('Is your `ORDER BY` clause correct?')

Ex().test_correct(check_result(), [
    order_by_clause,
    group_by_clause,
    avg_in_having,
    having_clause,
    from_clause,
    where_release_year,
    alias1,
    alias2,
    release_year,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

---
## All together now (2)

```yaml
type: NormalExercise
lang: sql
xp: 100
skills: 1
key: 0bbc6da34d
```

Great work! Now try another large query. This time, all in one go!

Remember, if you only want to return a certain number of results, you can use the `LIMIT` keyword to limit the number of rows returned

`@instructions`
Get the country, average budget, and average gross take of countries that have made more than 10 films. Order the result by country name, and limit the number of results displayed to 5. You should alias the averages as `avg_budget` and `avg_gross` respectively.


`@pre_exercise_code`
```{python}
connect('postgresql', 'films')
set_options(visible_tables = ['films'])
```

`@sample_code`
```{sql}
-- select country, average budget, average gross

-- from the films table

-- group by country 

-- where the country has a title count greater than 10

-- order by country

-- limit to only show 5 results

```

`@solution`
```{sql}
-- select country, average budget, average gross
SELECT country, AVG(budget) AS avg_budget, AVG(gross) AS avg_gross
-- from the films table
FROM films
-- group by country 
GROUP BY country
-- where the country has a title count greater than 10
HAVING COUNT(title) > 10
-- order by country
ORDER BY country
-- limit to only show 5 results
LIMIT 5;
```
`@hint`
```
SELECT ___, AVG(budget) AS avg_budget, ___(gross) AS avg_gross
FROM films
GROUP BY ___
HAVING COUNT(title) > 10
ORDER BY ___
LIMIT 5;
```

`@sct`
```{python}
sel = check_node('SelectStmt')

country = test_column('country', msg='Did you select the `country` column correctly?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')
where_clause = sel.check_field('where_clause').has_equal_ast('Is your `WHERE` clause correct?')
group_by = sel.check_field('group_by_clause').has_equal_ast('Is your `GROUP BY` clause correct?')
having_clause = sel.check_field('having_clause').has_equal_ast('Is your `HAVING` clause correct?')
order_by = sel.check_field('order_by_clause').has_equal_ast('Is your `ORDER BY` clause correct?')

alias1 = test_column('avg_budget', match='exact', msg='Are you aliasing `avg_budget` correctly?')
alias2 = test_column('avg_gross', match='exact', msg='Are you aliasing `avg_gross` correctly?')

avg_in_having = having_clause.check_node('Call').has_equal_ast('Are you correctly calling `AVG` on `budget` in your `HAVING` clause?')

limit_clause = sel.check_field('limit_clause').has_equal_ast('Did you `LIMIT` the number of results to `5`?')

Ex().test_correct(check_result(), [
    limit_clause,
    order_by,
    group_by,
    from_clause,
    having_clause,
    country,
    alias1,
    alias2,
    test_has_columns(),
    test_ncols(),
    test_error()
])
Ex().success_msg("Superb work on a selection saga! SELECT queries can get rather long, but breaking them down into individual clauses makes them easier to write.")
```

---
## A taste of things to come

```yaml
type: TabExercise
lang: sql
xp: 100
key: d101be707a
```

Congrats on making it to the end of the course! By now you should have a good understanding of the basics of SQL.

There's one more concept we're going to introduce. You may have noticed that all your results so far have been from just one table, e.g. `films` or `people`.

In the real world however, you will often want to query multiple tables. For example, what if you want to see the IMDB score for a particular movie?

In this case, you'd want to get the ID of the movie from the `films` table and then use it to get IMDB information from the `reviews` table. In SQL, this concept is known as a **join**, and a basic join is shown in the editor to the right.

The query in the editor gets the IMDB score for the film _To Kill a Mockingbird_! Cool right?

As you can see, joins are incredibly useful and important to understand for anyone using SQL.

We have a whole follow-up course dedicated to them called <a href="https://www.datacamp.com/courses/joining-data-in-postgresql">Joining Data in PostgreSQL</a> for you to hone your database skills further!

`@pre_exercise_code`
```{sql}
connect('postgresql', 'films')
set_options(visible_tables = ['films', 'reviews'])
```

`@sample_code`
```{sql}
SELECT title, imdb_score
FROM films
JOIN reviews
ON films.id = reviews.film_id
WHERE title = 'To Kill a Mockingbird';
```

***

```yaml
type: NormalExercise
key: 7c4fc7a484
xp: 30
```

`@instructions`
Submit the code in the editor and inspect the results.

`@hint`
Submit the code in the editor!

`@solution`
```{sql}
SELECT title, imdb_score
FROM films
JOIN reviews
ON films.id = reviews.film_id
WHERE title = 'To Kill a Mockingbird';
```

`@sct`
```{sql}
sel = check_node('SelectStmt')

title = test_column('title', msg='Did you select the `title` column correctly?')

imdb_score = test_column('imdb_score', msg='Did you select the `imdb_score` column correctly?')

from_clause = sel.check_field('from_clause').has_equal_ast('Is your `FROM` clause correct?')

where_clause = sel.check_field('where_clause').has_equal_ast('Is your `WHERE` clause correct?')

Ex().test_correct(check_result(), [
    from_clause,
    where_clause,
    title,
    imdb_score,
    test_has_columns(),
    test_ncols(),
    test_error()
])

```

***

```yaml
type: MultipleChoiceExercise
key: fc288db979
xp: 30
```

What rating does _To Kill a Mockingbird_ have on IMDB?

`@instructions`
- 8.1
- 8.4
- 7.7
- 9.3

`@hint`
Look at the query result tab!

`@sct`
```{python}
msg1 = 'Nope, look at the query results!'
correct = 'Correct!'

Ex().test_mc(2,[msg1, correct, msg1, msg1])
```
