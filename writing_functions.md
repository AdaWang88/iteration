Writing functions
================
Pengchen(Ada) Wang
2026-03-28

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1]  0.80320376 -1.38025762  1.68833214 -0.03035932  1.29644361  0.30132463
    ##  [7]  0.24816238 -0.09864372 -0.64342645  0.81485598  2.34345375  0.30983972
    ## [13] -1.80830002 -0.30970950  0.28354933 -2.19981713  0.56854683  0.20110867
    ## [19] -1.07013673  0.45667445 -0.70627711 -0.92993907 -0.07122035 -0.32068475
    ## [25]  0.18937866 -0.50020062 -1.38672322  0.60520370  0.93224896  0.41336904

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

    ##  [1]  0.80320376 -1.38025762  1.68833214 -0.03035932  1.29644361  0.30132463
    ##  [7]  0.24816238 -0.09864372 -0.64342645  0.81485598  2.34345375  0.30983972
    ## [13] -1.80830002 -0.30970950  0.28354933 -2.19981713  0.56854683  0.20110867
    ## [19] -1.07013673  0.45667445 -0.70627711 -0.92993907 -0.07122035 -0.32068475
    ## [25]  0.18937866 -0.50020062 -1.38672322  0.60520370  0.93224896  0.41336904

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
