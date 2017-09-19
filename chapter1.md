---
title       : Insert the chapter title here
description : Insert the chapter description here
attachments :
  slides_link : https://s3.amazonaws.com/assets.datacamp.com/course/teach/slides_example.pdf

---
## A really bad movie

```yaml
type: MultipleChoiceExercise
lang: r
xp: 50
skills: 1
key: 8dbdfc9ed5
```

Have a look at the plot that showed up in the viewer to the right. Which type of movie has the worst rating assigned to it?

`@instructions`
- Adventure
- Action
- Animation
- Comedy

`@hint`
Have a look at the plot. Which color does the point with the lowest rating have?

`@pre_exercise_code`
```{r}
# The pre exercise code runs code to initialize the user's workspace.
# You can use it to load packages, initialize datasets and draw a plot in the viewer

movies <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/course/introduction_to_r/movies.csv")

library(ggplot2)

ggplot(movies, aes(x = runtime, y = rating, col = genre)) + geom_point()
```

`@sct`
```{r}
# SCT written with testwhat: https://github.com/datacamp/testwhat/wiki

msg_bad <- "That is not correct!"
msg_success <- "Exactly! There seems to be a very bad action movie in the dataset."
test_mc(correct = 2, feedback_msgs = c(msg_bad, msg_success, msg_bad, msg_bad))
```

---

## More movies

```yaml
type: NormalExercise
lang: r
xp: 100
skills: 1
key: b22e553cba
```

In the previous exercise, you saw a dataset about movies. In this exercise, we'll have a look at yet another dataset about movies!

A dataset with a selection of movies, `movie_selection`, is available in the workspace.

`@instructions`
- Check out the structure of `movie_selection`.
- Select movies with a rating of 5 or higher. Assign the result to `good_movies`.
- Use `plot()` to  plot `good_movies$Run` on the x-axis, `good_movies$Rating` on the y-axis and set `col` to `good_movies$Genre`.

`@hint`
- Use `str()` for the first instruction.
- For the second instruction, you should use `...[movie_selection$Rating >= 5, ]`.
- For the plot, use `plot(x = ..., y = ..., col = ...)`.

`@pre_exercise_code`
```{r}
# You can also prepare your dataset in a specific way in the pre exercise code
load(url("https://s3.amazonaws.com/assets.datacamp.com/course/teach/movies.RData"))
movie_selection <- Movies[Movies$Genre %in% c("action", "animated", "comedy"), c("Genre", "Rating", "Run")]

# Clean up the environment
rm(Movies)
```

`@sample_code`
```{r}
# movie_selection is available in your workspace

# Check out the structure of movie_selection


# Select movies that have a rating of 5 or higher: good_movies


# Plot Run (i.e. run time) on the x axis, Rating on the y axis, and set the color using Genre

```

`@solution`
```{r}
# movie_selection is available in your workspace

# Check out the structure of movie_selection
str(movie_selection)

# Select movies that have a rating of 5 or higher: good_movies
good_movies <- movie_selection[movie_selection$Rating >= 5, ]

# Plot Run (i.e. run time) on the x axis, Rating on the y axis, and set the color using Genre
plot(good_movies$Run, good_movies$Rating, col = good_movies$Genre)
```

`@sct`
```{r}
# SCT written with testwhat: https://github.com/datacamp/testwhat/wiki

test_function("str", args = "object",
              not_called_msg = "You didn't call `str()`!",
              incorrect_msg = "You didn't call `str(object = ...)` with the correct argument, `object`.")

test_object("good_movies")

test_function("plot", args = "x")
test_function("plot", args = "y")
test_function("plot", args = "col")

test_error()

success_msg("Good work!")
```

---
## New subexercises

```yaml
type: TabExercise
lang: sql
xp: 100
key: ecc1838fc7
```

This is what new tab exercises look like.

`@pre_exercise_code`
```{python}
connect('postgresql', 'films')
set_options(visible_tables = ['films'])
```

`@sample_code`
```{sql}
-- Nothing here
```

***

```yaml
type: NormalExercise
xp: 30
```

`@instructions`
Get the title and release year for films released in the 90s.

`@solution`
```{sql}
SELECT title, release_year
FROM films
WHERE release_year >= 1990 AND release_year < 2000;
```

`@hint`
```
SELECT ___, ___
FROM ___
WHERE ___ >= 1990 AND ___ < 2000;
```

`@sct`
```{python}
sel = check_node('SelectStmt')

title = test_column('title', msg='Did you select the `title` column?')

release_year = test_column('release_year', msg='Did you select the `release_year` column?')

from_clause = sel.check_field('from_clause')

where_clause = sel.check_field('where_clause')

where_release_year1 = where_clause.has_equal_ast(sql='release_year >= 1990', start='expression', exact=False, msg='Did you check the `release_year` correctly in your `WHERE` clause?')

where_release_year2 = where_clause.has_equal_ast(sql='release_year < 2000', start='expression', exact=False, msg='Did you check the `release_year` correctly in your `WHERE` clause?')

Ex().test_correct(check_result(), [
    from_clause,
    where_release_year1,
    where_release_year2,
    title,
    release_year,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

***

```yaml
type: NormalExercise
xp: 30
```

`@instructions`
Now, build on your query to filter the records to only include French or Spanish language films.

`@solution`
```{sql}
SELECT title, release_year
FROM films
WHERE (release_year >= 1990 AND release_year < 2000)
AND (language = 'French' OR language = 'Spanish');
```

`@hint`
```
SELECT ___, ___
FROM ___
WHERE (___ >= 1990 AND ___ < 2000)
AND (___ = 'French' OR ___ = 'Spanish');
```

`@sct`
```{python}
sel = check_node('SelectStmt')

title = test_column('title', msg='Did you select the `title` column?')

release_year = test_column('release_year', msg='Did you select the `release_year` column?')

from_clause = sel.check_field('from_clause')

where_clause = sel.check_field('where_clause')

where_release_year1 = where_clause.has_equal_ast(sql='release_year >= 1990', start='expression', exact=False, msg='Did you check the `release_year` correctly in your `WHERE` clause?')

where_release_year2 = where_clause.has_equal_ast(sql='release_year < 2000', start='expression', exact=False, msg='Did you check the `release_year` correctly in your `WHERE` clause?')

where_language1 = where_clause.has_equal_ast(sql="language = 'French'", start='expression', exact=False, msg='Did you check the `language` correctly in your `WHERE` clause?')

where_language2 = where_clause.has_equal_ast(sql="language = 'Spanish'", start='expression', exact=False, msg='Did you check the `language` correctly in your `WHERE` clause?')


Ex().test_correct(check_result(), [
    from_clause,
    where_release_year1,
    where_release_year2,
    where_language1,
    where_language2,
    title,
    release_year,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```

***

```yaml
type: NormalExercise
xp: 30
```

`@instructions`
Finally, restrict the query to only return films that took in more than $2M gross.

`@solution`
```{sql}
SELECT title, release_year
FROM films
WHERE (release_year >= 1990 AND release_year < 2000)
AND (language = 'French' OR language = 'Spanish')
AND gross > 2000000;
```

`@hint`
```
SELECT ___, ___
FROM ___
WHERE (___ >= 1990 AND ___ < 2000)
AND (___ = '___' OR ___ = '___')
AND ___ > ___;
```

`@sct`
```{python}
sel = check_node('SelectStmt')

title = test_column('title', msg='Did you select the `title` column?')

release_year = test_column('release_year', msg='Did you select the `release_year` column?')

from_clause = sel.check_field('from_clause')

where_clause = sel.check_field('where_clause')

where_release_year1 = where_clause.has_equal_ast(sql='release_year >= 1990', start='expression', exact=False, msg='Did you check the `release_year` correctly?')

where_release_year2 = where_clause.has_equal_ast(sql='release_year < 2000', start='expression', exact=False, msg='Did you check the `release_year` correctly in your `WHERE` clause?')

where_language1 = where_clause.has_equal_ast(sql="language = 'French'", start='expression', exact=False, msg='Did you check the `language` correctly in your `WHERE` clause?')

where_language2 = where_clause.has_equal_ast(sql="language = 'Spanish'", start='expression', exact=False, msg='Did you check the `language` correctly in your `WHERE` clause?')

where_gross = where_clause.has_equal_ast(sql='gross > 2000000', start='expression', exact=False, msg='Did you check the `gross` correctly in your `WHERE` clause?')

Ex().test_correct(check_result(), [
    from_clause,
    where_release_year1,
    where_release_year2,
    where_language1,
    where_language2,
    where_gross,
    title,
    release_year,
    test_has_columns(),
    test_ncols(),
    test_error()
])
```
