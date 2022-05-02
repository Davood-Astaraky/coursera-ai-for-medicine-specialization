Week 1 Labs 2: Risk Scores - using R
================
Juan Li (based on python code on Github)
04/27/2022

Here, you'll get a chance to see the risk scores implemented as Python functions.

-   Atrial fibrillation: Chads-vasc score
-   Liver disease: MELD score
-   Heart disease: ASCVD score

Compute the chads-vasc risk score for atrial fibrillation.

-   Look for the `# TODO` comments to see which parts you will complete.

``` r
# Complete the function that calculates the chads-vasc score. 
# Look for the # TODO comments to see which sections you should fill in.

chads_vasc_score <- function(input_c, input_h, input_a2, input_d, input_s2, input_v, input_a, input_sc) {
  # congestive heart failure
    coef_c <- 1 
    
    # Coefficient for hypertension
    coef_h <- 1 
    
    # Coefficient for Age >= 75 years
    coef_a2 <- 2
    
    # Coefficient for diabetes mellitus
    coef_d <- 1
    
    # Coefficient for stroke
    coef_s2 <- 2
    
    # Coefficient for vascular disease
    coef_v <- 1
    
    # Coefficient for age 65 to 74 years
    coef_a <- 1
    
    # TODO Coefficient for female
    coef_sc <- 1
    
    # Calculate the risk score
    risk_score = (input_c * coef_c) +
                 (input_h * coef_h) +
                 (input_a2 * coef_a2) +
                 (input_d * coef_d) +
                 (input_s2 * coef_s2) +
                 (input_v * coef_v) +
                 (input_a * coef_a) +
                 (input_sc * coef_sc)
    
    return(risk_score)
}
```

## Calculate the risk score

Calculate the chads-vasc score for a patient who has the following attributes:

-   Congestive heart failure? No
-   Hypertension: yes
-   Age 75 or older: no
-   Diabetes mellitus: no
-   Stroke: no
-   Vascular disease: yes
-   Age 65 to 74: no
-   Female? : yes

``` r
# Calculate the patient's Chads-vasc risk score
tmp_c <- 0
tmp_h <- 1
tmp_a2 <- 0
tmp_d <- 0
tmp_s2 <- 0
tmp_v <- 1
tmp_a <- 0
tmp_sc <- 1

print(paste("The chads-vasc score for this patient is",
            chads_vasc_score(tmp_c, tmp_h, tmp_a2, tmp_d, tmp_s2, tmp_v, tmp_a, tmp_sc)))
# [1] "The chads-vasc score for this patient is 3"
```

### Expected output

The chads-vasc score $\\color{green}{\\text{for this}}$ patient is $\\color{green}{\\text{3}}$.

## Risk score for liver disease

Complete the implementation of the MELD score and use it to calculate the risk score for a particular patient.

-   Look for the `# TODO` comments to see which parts you will complete.

``` r
liver_disease_mortality <- function(input_creatine, input_bilirubin, input_inr) {
  # Calculate the probability of mortality given that the patient has liver disease. 
  # Parameters:
  #      Creatine: mg/dL
  #      Bilirubin: mg/dL
  #      INR: 
  
  # Coefficient values
  coef_creatine <- 0.957
  coef_bilirubin <- 0.378
  coef_inr <- 1.12
  intercept <- 0.643
    
  # Calculate the natural logarithm of input variables
  log_cre <- log(input_creatine)
  log_bil <- log(input_bilirubin)
    
  # TODO: Calculate the natural log of input_inr
  log_inr <- log(input_inr)
    
  # Compute output
  meld_score = (coef_creatine * log_cre) +
               (coef_bilirubin * log_bil ) +
               (coef_inr * log_inr) +
               intercept
    
  # TODO: Multiply meld_score by 10 to get the final risk score
  meld_score = meld_score * 10
    
  return(meld_score)
}
```

For a patient who has

-   Creatinine: 1 mg/dL
-   Bilirubin: 2 mg/dL
-   INR: 1.1

Calculate their MELD score

``` r
tmp_meld_score <- liver_disease_mortality(1.0, 2.0, 1.1)
print(paste("The patient's MELD score is:", round(tmp_meld_score,2)))
# [1] "The patient's MELD score is: 10.12"
```

### Expected output

The patient's MELD score is: $\\color{green}{\\text{10.12}}$.

## ASCVD Risk score for heart disease

Complete the function that calculates the ASCVD risk score!

-   Ln(Age), coefficient is 17.114
-   Ln(total cholesterol): coefficient is 0.94
-   Ln(HDL): coefficient is -18.920
-   Ln(Age) x Ln(HDL-C): coefficient is 4.475
-   Ln (Untreated systolic BP): coefficient is 27.820
-   Ln (Age) x Ln 10 (Untreated systolic BP): coefficient is -6.087
-   Current smoker (1 or 0): coefficient is 0.691
-   Diabetes (1 or 0): coefficient is 0.874

Remember that after you calculate the sum of the products (of inputs and coefficients), use this formula to get the risk score:
*R**i**s**k* = 1 − 0.9533<sup>*e*<sup>*s**u**m**P**r**o**d* − 86.61</sup></sup>

This is 0.9533 raised to the power of this expression: *e*<sup>*s**u**m**P**r**o**d* − 86.61</sup>, and not 0.9533 multiplied by that exponential.

-   Look for the `# TODO` comments to see which parts you will complete.

``` r
ascvd <- function(x_age, x_cho, x_hdl, x_sbp, x_smo, x_dia, verbose=FALSE) {
  # Atherosclerotic Cardiovascular Disease (ASCVD) Risk Estimator Plus 
  
  # Define the coefficients
  b_age <- 17.114
  b_cho <- 0.94
  b_hdl <- -18.92
  b_age_hdl <- 4.475
  b_sbp <- 27.82
  b_age_sbp <- -6.087
  b_smo <- 0.691
  b_dia <- 0.874
    
  # Calculate the sum of the products of inputs and coefficients
  sum_prod <-  b_age * log(x_age) + 
              b_cho * log(x_cho) + 
              b_hdl * log(x_hdl) + 
              b_age_hdl * log(x_age) * log(x_hdl) +
              b_sbp * log(x_sbp) +
              b_age_sbp * log(x_age) * log(x_sbp) +
              b_smo * x_smo + 
              b_dia * x_dia
  
  if (verbose){
    print(paste("log(x_age):", round(log(x_age),2)))
    print(paste("log(x_cho):", round(log(x_cho),2)))
    print(paste("log(x_hdl):", round(log(x_hdl),2)))
    print(paste("log(x_age) * log(x_hdl):", round(log(x_age) * log(x_hdl),2)))
    print(paste("log(x_sbp):", round(log(x_sbp),2)))
    print(paste("log(x_age) * log(x_sbp)", round(log(x_age) * log(x_sbp),2)))
    print(paste("sum_prod", round(sum_prod,2)))
  }  
  
  # TODO: Risk Score = 1 - (0.9533^( e^(sum - 86.61) ) )
  risk_score = 1 - 0.9533 ^ exp(sum_prod - 86.61)
    
  return(risk_score)
}
```

``` r
tmp_risk_score <- ascvd(x_age=55,
                      x_cho=213,
                      x_hdl=50,
                      x_sbp=120,
                      x_smo=0,
                      x_dia=0, 
                      verbose=TRUE
                      )
# [1] "log(x_age): 4.01"
# [1] "log(x_cho): 5.36"
# [1] "log(x_hdl): 3.91"
# [1] "log(x_age) * log(x_hdl): 15.68"
# [1] "log(x_sbp): 4.79"
# [1] "log(x_age) * log(x_sbp) 19.19"
# [1] "sum_prod 86.17"
print(paste("patient's ascvd risk score is", round(tmp_risk_score,2)))
# [1] "patient's ascvd risk score is 0.03"
```

### Expected output

patient's ascvd risk score is $\\color{green}{\\text{0.03}}$.

## Numpy and Pandas Operations

This section is not applied to R, so I'll skip this part.

## This is the end of this practice section.

Please continue on with the lecture videos!