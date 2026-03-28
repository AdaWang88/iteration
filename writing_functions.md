Writing functions
================
Pengchen(Ada) Wang
2026-03-28

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -1.445954449 -1.704619256  0.508881991  0.525748678 -1.983809958
    ##  [6]  1.253000992  0.005373491  0.522075892  1.035800308 -1.160554345
    ## [11]  1.950096571  0.686750214  0.629763929  1.195355448  0.563838954
    ## [16] -0.157180181 -1.833889767 -0.154221129  0.733171593  0.643006022
    ## [21] -0.566900644  0.151811368  0.949570856  0.102778809 -0.219590382
    ## [26] -0.141686110 -0.195477751 -0.171692655 -1.759434625  0.037986135

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

    ##  [1] -1.445954449 -1.704619256  0.508881991  0.525748678 -1.983809958
    ##  [6]  1.253000992  0.005373491  0.522075892  1.035800308 -1.160554345
    ## [11]  1.950096571  0.686750214  0.629763929  1.195355448  0.563838954
    ## [16] -0.157180181 -1.833889767 -0.154221129  0.733171593  0.643006022
    ## [21] -0.566900644  0.151811368  0.949570856  0.102778809 -0.219590382
    ## [26] -0.141686110 -0.195477751 -0.171692655 -1.759434625  0.037986135

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
    ## 1  2.78  3.93

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
    ## 1  4.73  3.05

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
    ## 1  6.17  3.09

``` r
sim_mean_sd(mu = 6, sample_size = 100, sigma = 3)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.74  2.73
