
<!-- README.md is generated from README.Rmd. Please edit that file -->

# inops

R package implementing additional infix operators that fall into 3
distinct types:

  - **set operators** for dealing with sets.
  - **range operators** for dealing with ranges.
  - **regular expression operators** for dealing with regular
    expressions.

Install using the `remotes` package:

    remotes::install_github("moodymudskipper/inops")

## Operators

Introduction to operator behaviour and design.

### Form

All operators have the same form composed of two distinct parts:
`%<operation><type>%`.

  - `[operation]` specifies the performed operation and can be either
    `in`, `out,`\[in`,`\[out\].
  - `[type]` specifies the type of operation and can be one of `{}`,
    `[]`, `()`, `[)`, `(]`, `~`, `~p`, `~f`.

To understand what each combination does see the table below.

### Behaviour

The operators implemented here try to be consistent with the default
comparison operators like `==` and `<`. Therefore in some scenarios
their behaviour differs from `%in%`. For instance:

1)  `%in{}%` can be used on on data frames.

<!-- end list -->

``` r
df1 <- data.frame(a = 1:3, b = 2:4, c=letters[1:3])
df1 == 2
#>          a     b     c
#> [1,] FALSE  TRUE FALSE
#> [2,]  TRUE FALSE FALSE
#> [3,] FALSE FALSE FALSE
df1 %in% 2
#> [1] FALSE FALSE FALSE
df1 %in{}% 2
#>          a     b     c
#> [1,] FALSE  TRUE FALSE
#> [2,]  TRUE FALSE FALSE
#> [3,] FALSE FALSE FALSE
df1 %in{}% 2:3
#>          a     b     c
#> [1,] FALSE  TRUE FALSE
#> [2,]  TRUE  TRUE FALSE
#> [3,]  TRUE FALSE FALSE
```

2)  missing values are not considered as not matching.

<!-- end list -->

``` r
NA == 1
#> [1] NA
NA %in% 1
#> [1] FALSE
NA %in{}% 1
#> [1] NA
c(1, NA, 3) %in{}% 1:10
#> [1] TRUE   NA TRUE
```

### Operations

Operators permit three distinct operations:

1.  Detect: `x %in{}% set`
2.  Subset: `x %[in{}% set`
3.  Replace: `x %in{}% set <- value`

## Operator List

Below is a full table listing all the implemented operators along with
their usage examples.

    |-----------|-----------------------------------------------------------|---------------------------------------------|
    |  Form     |                    Description                            |             Example                         |
    |-----------|-----------------------------------------------------------|---------------------------------------------|
    | %in{}%    | is element inside a set                                   | letters %in{}% "a"                          |
    | %in[]%    | is element inside a closed interval                       | rivers %in[]% c(500,1000)                   |
    | %in()%    | is element inside an open interval                        | rivers %in()% c(500,1000)                   |
    | %in[)%    | is element inside an interval open on the right           | rivers %in[)% c(500,1000)                   |
    | %in(]%    | is element inside an interval open on the left            | rivers %in(]% c(500,1000)                   |
    | %in~%     | does element match a regular expression                   | state.region %in~% "^N"                     |
    | %in~p%    | does element match a regular perl expression              | state.region %in~p% "^N"                    |
    | %in~f%    | does element match a regular fixed expression             | state.region %in~f% "North"                 |
    | %out%     | is element outside a set (same as ! x %in% y)             | letters %out% "a"                           |
    | %out{}%   | is element outside a set                                  | letters %out{}% "a"                         |
    | %out[]%   | is element outside a closed interval                      | rivers %out[]% c(500,1000)                  |
    | %out()%   | is element outside an open interval                       | rivers %out()% c(500,1000)                  |
    | %out[)%   | is element outside an interval open on the right          | rivers %out[)% c(500,1000)                  |
    | %out(]%   | is element outside an interval open on the left           | rivers %out(]% c(500,1000)                  |
    | %out~%    | does element not match a regular expression               | state.region %out~% "^N"                    |
    | %out~p%   | does element not match a regular perl expression          | state.region %out~p% "^N"                   |
    | %out~f%   | does element not match a regular fixed expression         | state.region %out~f% "North"                |
    | %[in{}%   | select elements inside a set                              | letters %[in{}% "a"                         |
    | %[in[]%   | select elements inside a closed interval                  | rivers %[in[]% c(500,1000)                  |
    | %[in()%   | select elements inside an open interval                   | rivers %[in()% c(500,1000)                  |
    | %[in[)%   | select elements inside an interval open on the right      | rivers %[in[)% c(500,1000)                  |
    | %[in(]%   | select elements inside an interval open on the left       | rivers %[in(]% c(500,1000)                  |
    | %[in~%    | select elements matching a regular expression             | state.region %[in~% "^N"                    |
    | %[in~p%   | select elements matching a regular perl expression        | state.region %[in~p% "^N"                   |
    | %[in~f%   | select elements matching a regular fixed expression       | state.region %[in~f% "North"                |
    | %[out%    | select elements outside a set                             | letters %[out% "a"                          |
    | %[out{}%  | select elements outside a set                             | letters %[out{}% "a"                        |
    | %[out[]%  | select elements outside a closed interval                 | rivers %[out[]% c(500,1000)                 |
    | %[out()%  | select elements outside an open interval                  | rivers %[out()% c(500,1000)                 |
    | %[out[)%  | select elements outside an interval open on the right     | rivers %[out[)% c(500,1000)                 |
    | %[out(]%  | select elements outside an interval open on the left      | rivers %[out(]% c(500,1000)                 |
    | %[out~%   | select elements not matching a regular expression         | state.region %[out~% "^N"                   |
    | %[out~p%  | select elements not matching a regular perl expression    | state.region %[out~p% "^N"                  |
    | %[out~f%  | select elements not matching a regular fixed expression   | state.region %[out~f% "North"               |
    | %==<-%    | change elements equal to the provided value               | letters == "a" <- "A"                       |
    | %!=<-%    | change elements not equal to the provided value           | letters != "a" <- "not A"                   |
    | %><-%     | change elements greater than the provided value           | rivers > 1000 <- 1000                       |
    | %<<-%     | change elements lower than the provided value             | rivers < 500  <- 500                        |
    | %>=<-%    | change elements greater or equal to the provided value    | rivers >= 1000 <- 1000                      |
    | %<=<-%    | change elements lower or equal to the provided value      | rivers <= 500 <- 500                        |
    | %in{}<-%  | change elements inside a set                              | letters %in{}% "a" <- "A"                   |
    | %in[]<-%  | change elements inside a closed interval                  | rivers %in[]% c(500,1000) <- 750            |
    | %in()<-%  | change elements inside an open interval                   | rivers %in()% c(500,1000) <- 750            |
    | %in[)<-%  | change elements inside an interval open on the right      | rivers %in[)% c(500,1000) <- 750            |
    | %in(]<-%  | change elements inside an interval open on the left       | rivers %in(]% c(500,1000) <- 750            |
    | %in~<-%   | change elements matching a regular expression             | state.region %in~% "^N" <- "North"          |
    | %in~p<-%  | change elements matching a regular perl expression        | state.region %in~p% "^N" <- "North"         |
    | %in~f<-%  | change elements matching a regular fixed expression       | state.region %in~f% "North" <- "North"      |
    | %out<-%   | change elements outside a set                             | letters %out% "a" <- "not A"                |
    | %out{}<-% | change elements outside a set                             | letters %out{}% "a" <- "not A"              |
    | %out[]<-% | change elements outside a closed interval                 | rivers %out[]% c(500,1000) <- NA            |
    | %out()<-% | change elements outside an open interval                  | rivers %out()% c(500,1000) <- NA            |
    | %out[)<-% | change elements outside an interval open on the right     | rivers %out[)% c(500,1000) <- NA            |
    | %out(]<-% | change elements outside an interval open on the left      | rivers %out(]% c(500,1000) <- NA            |
    | %out~<-%  | change elements not matching a regular expression         | state.region %out~% "^N" <- "not North"     |
    | %out~p<-% | change elements not matching a regular perl expression    | state.region %out~p% "^N" <- "not North"    |
    | %out~f<-% | change elements not matching a regular fixed expression   | state.region %out~f% "North" <- "not North" |
    |-----------|-----------------------------------------------------------|---------------------------------------------|

## Notes

To give an assignment counterpart to `<` we had to overload the `<<-`
operator, which explains the message when attaching the package. This
doesn’t affect the behavior of the `<<-` assignments.
