Week 1 Labs 4: Concordance index - using R
================
Juan Li (based on python code on Github)
04/27/2022

In this week's graded assignment, you will implement the concordance index (c-index). To get some practice with what you've seen in lecture, and to prepare for this week's assignment, you will write code to find permissible pairs, concordant pairs, and risk ties.

First start by importing packages and generating a small dataset. The data is small enough that you can visually check the pairs of patients.

## Define the outcome `y`

-   You will let `y` refer to the actual health outcome of the patient.
-   1 indicates disease, 0 indicates health (normal)

``` r
# define 'y', the outcome of the patient
# y <- c(0,0,1,1, 0)

# Note for some reason, in the original Python file, the length of y (see above) is not identical 
# to the length of risk_score below. I changed y so they have same length.
y <- c(0,0,1,1)
y
# [1] 0 0 1 1
```

## Define the risk scores

Define some risk scores that some model might produce for each patient. Normally, you would run the patient features through a risk model to create these risk scores. For practice, you will use the following values in the next cell.

``` r
# Define the risk scores for each patient
risk_score     <- c(2.2, 3.3, 4.4, 4.4)
risk_score
# [1] 2.2 3.3 4.4 4.4
```

## Identify a permissible pair

A pair of patients is permissible if their outcomes are different. Use code to compare the labels.

``` r
# Check patients 1 and 2 make a permissible pair.
if (y[1] != y[2]){
  print(paste("y[1]=",y[1], " and y[2]=", y[2], " is a permissible pair", sep = ""))
} else{
  print(paste("y[1]=",y[1], " and y[2]=", y[2], " is not a permissible pair", sep = ""))
}
# [1] "y[1]=0 and y[2]=0 is not a permissible pair"
```

``` r
# Check patients 1 and 3 make a permissible pair.
if (y[1] != y[3]){
  print(paste("y[1]=",y[1], " and y[3]=", y[3], " is a permissible pair", sep = ""))
} else{
  print(paste("y[1]=",y[1], " and y[3]=", y[3], " is not a permissible pair", sep = ""))
}
# [1] "y[1]=0 and y[3]=1 is a permissible pair"
```

## Check for risk ties

For permissible pairs, check if they have the same risk score

``` r
# Check if patients 3 and 4 make a risk tie
if (risk_score[3] == risk_score[4]){
  print(paste("risk_score[3]=",risk_score[3], " and risk_score[4]=", risk_score[4], " have a risk tie", sep = ""))
} else{
  print(paste("risk_score[3]=",risk_score[3], " and risk_score[4]=", risk_score[4], " DO NOT have a risk tie", sep = ""))
}
# [1] "risk_score[3]=4.4 and risk_score[4]=4.4 have a risk tie"
```

## Concordant pairs

-   Check if a permissible pair is also a concordant pair
-   You'll check one case, where patient 2 is healthy and patient 3 has the disease.

``` r
# Check if patient 2 and 3 make a concordant pair
if (y[2] == 0 & y[3] == 1){
  if (risk_score[2] < risk_score[3]){
    print("patient 2 and 3 is a concordant pair")
  }
}
# [1] "patient 2 and 3 is a concordant pair"
```

-   Note that you checked the situation where patient 2 is healthy and patient 3 has the disease.
-   You should also check the other situation where patient 2 has the disease and patient 3 is healthy.

You'll practice implementing the complete algorithm for c-index in this week's assignment!

## This is the end of this practice section.

Please continue on with the lecture videos!