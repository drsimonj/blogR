
<img src="https://github.com/drsimonj/twidlr/raw/master/man/figures/logo.png" align="right" />

[@drsimonj](https://twitter.com/drsimonj) here to introduce my latest tidy-modelling package, "[twidlr](https://github.com/drsimonj/twidlr)". Its aim is to give modelling functions a consistent **data.frame-first formula-second API**.

Instead of writing model functions like this:

``` r
lm(hp ~ ., mtcars)

library(xgboost)
xgboost(as.matrix(mtcars[names(mtcars) != "hp"]), mtcars$hp)

library(glmnet)
glmnet(model.matrix(Petal.Length ~ Sepal.Width * Species, iris), iris$Petal.Length)
```

Write them like this:

``` r
library(twidlr)

lm(mtcars, hp ~ .)
xgboost(mtcars, hp ~ ., nrounds = 5)
glmnet(iris, Petal.Length ~ Sepal.Width * Species)
```

Or like this:

``` r
library(twidlr)

mtcars %>% lm(hp ~ .)
mtcars %>% xgboost(hp ~ ., nrounds = 5)
iris   %>% glmnet(Petal.Length ~ Sepal.Width * Species)
```

The problem
-----------

R model APIs are inconsistent and messy.

Some models want a formulas and data frames. For example, linear regression:

``` r
lm(hp ~ ., mtcars)
```

Some want vectors and matrices. For example, gradient-boosted decision trees:

``` r
library(xgboost)

y <- mtcars$hp
x <- as.matrix(mtcars[names(mtcars) != "hp"])

xgboost(x, y, nrounds = 5)
```

Some want you to work! For example, interactions and dummy-coded variables with generalized linear models:

``` r
library(glmnet)

y <- iris$Petal.Length
x <- model.matrix(Petal.Length ~ Sepal.Width * Sepal.Length + Species, iris)

glmnet(x, y)
```

And that's just the obvious stuff!

~ twidlr
--------

twidlr helps to solve these sorts of issues by exposing model functions with a consistent data.frame-first formula-second interface.

Load twidlr and use the model functions you know and love by passing them a data frame, a formula, and any additional arguments!

To demonstrate, compared to above:

``` r
library(twidlr)

lm(mtcars, hp ~ .)
xgboost(mtcars, hp ~ ., nrounds = 5)
glmnet(iris, Petal.Length ~ Sepal.Width * Sepal.Length + Species)
```

Take home messages
------------------

-   twidlr makes model APIs consistent and tidy
-   twidlr let's you pipe data frames
-   twidlr leverages formula operators
-   twidlr loads all your models functions (no need to load multiple packages)

Also, it's super easy to contribute. So if your favourite model isn't listed [here](https://github.com/drsimonj/twidlr), fork [twidlr on GitHub](https://github.com/drsimonj/twidlr) and add it to help improve modelling in R! Advice for contributing can be found [here](https://github.com/drsimonj/twidlr/blob/master/CONTRIBUTING.md).

Sign off
--------

Thanks for reading and I hope this was useful for you.

For updates of recent blog posts, follow [@drsimonj](https://twitter.com/drsimonj) on Twitter, or email me at <drsimonjackson@gmail.com> to get in touch.

If you'd like the code that produced this blog, check out the [blogR GitHub repository](https://github.com/drsimonj/blogR).
