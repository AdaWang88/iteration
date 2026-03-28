Writing functions
================
Pengchen(Ada) Wang
2026-03-28

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -0.88775449 -0.45892175  1.59061293 -0.23355875 -1.39250808  0.57422116
    ##  [7] -0.12036266  0.26424709  1.43674696 -0.84443038 -1.19116204  0.01349074
    ## [13]  0.19675641  1.39469545  0.23637686 -0.11758283 -1.20669812 -2.08889938
    ## [19] -0.63430804  0.75545220  0.37330781  0.17635918 -0.48071260  0.36467458
    ## [25]  0.29198505 -0.83275537  1.33664944 -1.53218424  1.39737843  1.61888444

I want a function to compute z-scores

``` r
z_scores = function(x) {
  
  if(!is.numeric(x)) {
    stop("Input must be numeric")
  }
  
  if(length(x) < 3) {
    stop("Input must have at least three numbers")
  }
  
  z = (x - mean(x)) / sd(x)
       
  return(z)
}

z_scores(x_vec)
```

    ##  [1] -0.88775449 -0.45892175  1.59061293 -0.23355875 -1.39250808  0.57422116
    ##  [7] -0.12036266  0.26424709  1.43674696 -0.84443038 -1.19116204  0.01349074
    ## [13]  0.19675641  1.39469545  0.23637686 -0.11758283 -1.20669812 -2.08889938
    ## [19] -0.63430804  0.75545220  0.37330781  0.17635918 -0.48071260  0.36467458
    ## [25]  0.29198505 -0.83275537  1.33664944 -1.53218424  1.39737843  1.61888444

Try my function on some other things. These should give errors.

``` r
z_scores(3)
```

    ## Error in `z_scores()`:
    ## ! Input must have at least three numbers

``` r
z_scores("my name is ada")
```

    ## Error in `z_scores()`:
    ## ! Input must be numeric

``` r
z_scores(mtcars)
```

    ## Error in `z_scores()`:
    ## ! Input must be numeric

``` r
z_scores(c(TRUE, TRUE, FALSE, TRUE))
```

    ## Error in `z_scores()`:
    ## ! Input must be numeric

## Multiple outputs

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

Check that the function works.

``` r
x_vec = rnorm(100, mean = 3, sd = 4)
mean_and_sd(x_vec)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.11  3.97

## Multiple inputs

``` r
sim_data =
  tibble(
    x = rnorm(100, mean = 4, sd = 3)
  )

sim_data %>% 
  summarize(
    mean = mean(x),
    sd = sd(x)
  )
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  4.08  2.99

``` r
sim_mean_sd = function(sample_size, mu, sigma) {
  sim_data =
  tibble(
    x = rnorm(n = sample_size, mean = mu, sd = sigma)
  )

sim_data %>% 
  summarize(
    mean = mean(x),
    sd = sd(x)
  )
}

sim_mean_sd(sample_size = 100, mu = 6, sigma = 3)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  6.21  2.74

``` r
sim_mean_sd(mu = 6, sample_size = 100, sigma = 3)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.96  2.84

## Let’s review Napolean Dynamite

## Mean scoping example

``` r
f = function(x) {
  z = x + y
  z
}

x = 1
y = 2

f(x = y)
```

    ## [1] 4

## Functions as arguments

``` r
my_summary = function(x, summ_func) {
  
  summ_func(x)
  
}

x_vec = rnorm(100, 3, 7)

mean(x_vec)
```

    ## [1] 3.505975

``` r
median(x_vec)
```

    ## [1] 4.407154

``` r
my_summary(x_vec, sd)
```

    ## [1] 6.59607
