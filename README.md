
<!-- README.md is generated from README.Rmd. Please edit that file -->

# inops

Package implementing additional infix operators for R.

Implemented operators provide 3 distinct operations: **detection**,
**subsetting**, and **replacement**. And work with 3 different value
types: **sets**, **intervals**, and **regular expressions**.

Install using the `remotes` package:

    remotes::install_github("moodymudskipper/inops")

## Operators

Introduction to operator behaviour and design.

### Form

All operators have the same form composed of two distinct parts:
`%<operation><type>%`.

  - `[operation]` specifies the performed operation and can be either
    `in`, `out,`\[in`,`\[out\`.
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

Below is a full list of all the implemented operators along with their
usage examples.

### Detection Operators

    |-----------|-----------------------------------------------------------|------------------------------|
    |  Form     |                    Description                            |            Call              |
    |-----------|-----------------------------------------------------------|------------------------------|
    | %in{}%    | is element inside a set                                   | x %in{}% set                 |
    | %in[]%    | is element inside a closed interval                       | x %in[]% interval            |
    | %in()%    | is element inside an open interval                        | x %in()% interval            |
    | %in[)%    | is element inside an interval open on the right           | x %in[)% interval            |
    | %in(]%    | is element inside an interval open on the left            | x %in(]% interval            |
    | %in~%     | does element match a regular expression                   | x %in~% pattern              |
    | %in~p%    | does element match a regular perl expression              | x %in~p% pattern             |
    | %in~f%    | does element match a regular fixed expression             | x %in~f% pattern             |
    | %out%     | is element outside a set (same as ! x %in% y)             | x %out% set                  |
    | %out{}%   | is element outside a set                                  | x %out{}% set                |
    | %out[]%   | is element outside a closed interval                      | x %out[]% interval           |
    | %out()%   | is element outside an open interval                       | x %out()% interval           |
    | %out[)%   | is element outside an interval open on the right          | x %out[)% interval           |
    | %out(]%   | is element outside an interval open on the left           | x %out(]% interval           |
    | %out~%    | does element not match a regular expression               | x %out~% pattern             |
    | %out~p%   | does element not match a regular perl expression          | x %out~p% pattern            |
    | %out~f%   | does element not match a regular fixed expression         | x %out~f% pattern            |
    |-----------|-----------------------------------------------------------|------------------------------|

### Subsetting Operators

    |-----------|-----------------------------------------------------------|------------------------------|
    |  Form     |                    Description                            |            Call              |
    |-----------|-----------------------------------------------------------|------------------------------|
    | %[in{}%   | select elements inside a set                              | x %[in{}% set                |
    | %[in[]%   | select elements inside a closed interval                  | x %[in[]% interval           |
    | %[in()%   | select elements inside an open interval                   | x %[in()% interval           |
    | %[in[)%   | select elements inside an interval open on the right      | x %[in[)% interval           |
    | %[in(]%   | select elements inside an interval open on the left       | x %[in(]% interval           |
    | %[in~%    | select elements matching a regular expression             | x %[in~% pattern             |
    | %[in~p%   | select elements matching a regular perl expression        | x %[in~p% pattern            |
    | %[in~f%   | select elements matching a regular fixed expression       | x %[in~f% pattern            |
    | %[out%    | select elements outside a set                             | x %[out%  set                |
    | %[out{}%  | select elements outside a set                             | x %[out{}%  set              |
    | %[out[]%  | select elements outside a closed interval                 | x %[out[]% interval          |
    | %[out()%  | select elements outside an open interval                  | x %[out()% interval          |
    | %[out[)%  | select elements outside an interval open on the right     | x %[out[)% interval          |
    | %[out(]%  | select elements outside an interval open on the left      | x %[out(]% interval          |
    | %[out~%   | select elements not matching a regular expression         | x %[out~% pattern            |
    | %[out~p%  | select elements not matching a regular perl expression    | x %[out~p% pattern           |
    | %[out~f%  | select elements not matching a regular fixed expression   | x %[out~f% pattern           |
    |-----------|-----------------------------------------------------------|------------------------------|

### Replacement Operators

    |-----------|-----------------------------------------------------------|------------------------------|
    |  Form     |                    Description                            |            Call              |
    |-----------|-----------------------------------------------------------|------------------------------|
    | %==<-%    | change elements equal to the provided value               | x == element <- value        |
    | %!=<-%    | change elements not equal to the provided value           | x != element <- value        |
    | %><-%     | change elements greater than the provided value           | x > number <- value          |
    | %<<-%     | change elements lower than the provided value             | x < number  <- value         |
    | %>=<-%    | change elements greater or equal to the provided value    | x >= number <- value         |
    | %<=<-%    | change elements lower or equal to the provided value      | x <= number <- value         |
    | %in{}<-%  | change elements inside a set                              | x %in{}% set <- value        |
    | %in[]<-%  | change elements inside a closed interval                  | x %in[]% interval <- value   |
    | %in()<-%  | change elements inside an open interval                   | x %in()% interval <- value   |
    | %in[)<-%  | change elements inside an interval open on the right      | x %in[)% interval <- value   |
    | %in(]<-%  | change elements inside an interval open on the left       | x %in(]% interval <- value   |
    | %in~<-%   | change elements matching a regular expression             | x %in~% pattern <- value     |
    | %in~p<-%  | change elements matching a regular perl expression        | x %in~p% pattern <- value    |
    | %in~f<-%  | change elements matching a regular fixed expression       | x %in~f% pattern <- value    |
    | %out<-%   | change elements outside a set                             | x %out% set <- value         |
    | %out{}<-% | change elements outside a set                             | x %out{}% set <- value       |
    | %out[]<-% | change elements outside a closed interval                 | x %out[]% interval <- value  |
    | %out()<-% | change elements outside an open interval                  | x %out()% interval <- value  |
    | %out[)<-% | change elements outside an interval open on the right     | x %out[)% interval <- value  |
    | %out(]<-% | change elements outside an interval open on the left      | x %out(]% interval <- value  |
    | %out~<-%  | change elements not matching a regular expression         | x %out~% pattern <- value    |
    | %out~p<-% | change elements not matching a regular perl expression    | x %out~p% pattern <- value   |
    | %out~f<-% | change elements not matching a regular fixed expression   | x %out~f% pattern <- value   |
    |-----------|-----------------------------------------------------------|------------------------------|

## Notes

To give an assignment counterpart to `<` we had to overload the `<<-`
operator, which explains the message when attaching the package. This
doesn’t affect the behavior of the `<<-` assignments.
