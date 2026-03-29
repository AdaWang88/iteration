Iteration and Listcols
================
Pengchen(Ada) Wang
2026-03-28

## Lists

You can put anything in a list.

``` r
l = list(
  vec_numeric = 5:8,
  vec_logical = c(TRUE, TRUE, FALSE, TRUE, FALSE, FALSE),
  mat = matrix(1:8, nrow = 2, ncol = 4),
  summary = summary(rnorm(100))
)
```

``` r
l
```

    ## $vec_numeric
    ## [1] 5 6 7 8
    ## 
    ## $vec_logical
    ## [1]  TRUE  TRUE FALSE  TRUE FALSE FALSE
    ## 
    ## $mat
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    3    5    7
    ## [2,]    2    4    6    8
    ## 
    ## $summary
    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -2.54701 -0.56454 -0.11798 -0.07538  0.56872  2.12478

``` r
l$vec_numeric
```

    ## [1] 5 6 7 8

``` r
l[[1]]
```

    ## [1] 5 6 7 8

``` r
mean(l[["vec_numeric"]])
```

    ## [1] 6.5

## `for` loop

Create a new list.

``` r
list_norm =
  list(
    a = rnorm(20, mean = 3, sd = 1),
    b = rnorm(30, mean = 0, sd = 5),
    c = rnorm(40, mean = 10, sd = .2),
    d = rnorm(20, mean = -3, sd = 1)
  )
```

``` r
list_norm[[1]]
```

    ##  [1] 2.639986 2.940531 1.133323 3.856512 3.697060 3.752068 3.034301 3.868506
    ##  [9] 5.050656 2.151696 3.789657 3.546367 2.002830 2.984088 4.566430 3.136635
    ## [17] 3.275795 2.782741 4.842731 3.819973

Pause and get my old function.

``` r
mean_and_sd = function(x) {
  
  if(!is.numeric(x)) {
    stop("Input must be numeric")
  }
  
  if(length(x) < 3) {
    stop("Input must have at least three numbers")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)
  
  tibble(
    mean = mean_x,
    sd = sd_x
  )
      
}
```

I can apply that function to each list element.

``` r
mean_and_sd(list_norm[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.34 0.954

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 × 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 0.0348  5.35

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.189

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.67 0.825

Let’s use a for loop:

``` r
output = vector("list", length = 4)

for(i in 1:4) {
  
  output[[i]] = mean_and_sd(list_norm[[i]])

}
```

## Let’s try map!

``` r
output = map(list_norm, mean_and_sd)
```

What if you want a different function…?

``` r
output = map(list_norm, IQR)
```

``` r
output = map_dbl(list_norm, median)
```

``` r
output = map_df(list_norm, mean_and_sd, .id = "input")
```

## List columns!

``` r
listcol_df =
  tibble(
    name = c("a", "b", "c", "d"),
    samp = list_norm
  )
```

``` r
listcol_df %>% pull(name)
```

    ## [1] "a" "b" "c" "d"

``` r
listcol_df %>% pull(samp)
```

    ## $a
    ##  [1] 2.639986 2.940531 1.133323 3.856512 3.697060 3.752068 3.034301 3.868506
    ##  [9] 5.050656 2.151696 3.789657 3.546367 2.002830 2.984088 4.566430 3.136635
    ## [17] 3.275795 2.782741 4.842731 3.819973
    ## 
    ## $b
    ##  [1]  14.2307111   2.8088893  -2.1398521  -7.2529267   2.9671836  -4.2442006
    ##  [7]  -2.0553787   1.9175551  -1.6389734   2.3839861   9.6511233   0.9781872
    ## [13]   2.2117859  -4.7232588   6.8232295  -2.6740106  -0.9050227  -2.1452460
    ## [19]  -6.1503196   2.8495936   2.0385215  -5.3717083  -3.9467918   3.4056452
    ## [25]  -4.1728148   4.0590945   2.2018406 -11.8224307  -4.3218217   6.0803059
    ## 
    ## $c
    ##  [1] 10.020518 10.188726 10.251337 10.000399 10.052177  9.975822 10.011916
    ##  [8]  9.776797  9.939050  9.974203 10.093345  9.904159 10.333603 10.074653
    ## [15]  9.951958 10.223559  9.929029  9.963730  9.935214 10.276358 10.346815
    ## [22]  9.787318  9.938529 10.275538  9.993229 10.288826 10.014190 10.015936
    ## [29]  9.553200 10.437855  9.968465 10.134392  9.914138 10.267222  9.933709
    ## [36]  9.856819  9.839189  9.798534  9.802095  9.834792
    ## 
    ## $d
    ##  [1] -2.090032 -2.454244 -3.563690 -2.799706 -3.532407 -1.097807 -4.647954
    ##  [8] -2.685071 -1.940156 -3.517330 -3.514934 -3.131194 -2.213671 -2.423069
    ## [15] -2.022990 -1.953683 -2.554761 -1.620614 -2.635321 -3.018369

``` r
listcol_df %>% filter(name == "a")
```

    ## # A tibble: 1 × 2
    ##   name  samp        
    ##   <chr> <named list>
    ## 1 a     <dbl [20]>

``` r
mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.34 0.954

``` r
mean_and_sd(listcol_df$samp[[2]])
```

    ## # A tibble: 1 × 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 0.0348  5.35

Can I just … map?

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.34 0.954
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 0.0348  5.35
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.189
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.67 0.825

So … can I add a list column?

``` r
listcol_df = 
listcol_df %>% 
  mutate(
    summary = map(samp, mean_and_sd),
    median = map_dbl(samp, median))
```
