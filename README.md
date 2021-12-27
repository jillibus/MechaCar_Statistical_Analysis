# MechaCar StatisticalAnalysis

![logo](images/module-15_logo.png)

## Overview
A few weeks after starting his new role, Jeremy is approached by upper management about a special project. AutosRUs’ newest prototype, the MechaCar, is suffering from production troubles that are blocking the manufacturing team’s progress. AutosRUs’ upper management has called on Jeremy and the data analytics team to review the production data for insights that may help the manufacturing team.

In this challenge, you’ll help Jeremy and the data analytics team do the following:

Perform multiple linear regression analysis to identify which variables in the dataset predict the mpg of MechaCar prototypes
Collect summary statistics on the pounds per square inch (PSI) of the suspension coils from the manufacturing lots
Run t-tests to determine if the manufacturing lots are statistically different from the mean population
Design a statistical study to compare vehicle performance of the MechaCar vehicles against vehicles from other manufacturers. For each statistical analysis, you’ll write a summary interpretation of the findings.

## Resources
* Data Sources: MechaCar_mpg.csv, Suspension_Coil.csv
* Software: R, Python 3.7

## GitHub Application Link
<a href="https://jillibus.github.io/MechaCar_Statistical_Analysis/">MechaCar Statistical Analysis</a>

## RESULTS
### Deliverable 1: Linear Regression to Predict MPG
- The non-random Variance was based on the vehicles: _weight_, _spoiler angle_ and if the vehicle is equiped with _AWD (all wheel drive)_. 
- The maximum random variance was provided by the vehicle's _ground_clearance_ and _vehicle_length_.
- Since p-value ended up being less than zero (5.35e-11), the slope is not equal to zero.
- The R squared value is 71.49%, which implies that roughly 72% of the time, the predictions will be correct using this linear model.

```
Call:
lm(formula = mpg ~ vehicle_length + vehicle_weight + spoiler_angle + 
    ground_clearance + AWD, data = mechacar_df)

Coefficients:
     (Intercept)    vehicle_length    vehicle_weight     spoiler_angle  ground_clearance               AWD  
      -1.040e+02         6.267e+00         1.245e-03         6.877e-02         3.546e+00        -3.411e+00  
      
      
> # Generate summary statistics
> summary(lm(mpg ~ vehicle_length + vehicle_weight + spoiler_angle + ground_clearance + AWD, data = mechacar_df))

Call:
lm(formula = mpg ~ vehicle_length + vehicle_weight + spoiler_angle + 
    ground_clearance + AWD, data = mechacar_df)

Residuals:
     Min       1Q   Median       3Q      Max 
-19.4701  -4.4994  -0.0692   5.4433  18.5849 

Coefficients:
                   Estimate Std. Error t value Pr(>|t|)    
(Intercept)      -1.040e+02  1.585e+01  -6.559 5.08e-08 ***
vehicle_length    6.267e+00  6.553e-01   9.563 2.60e-12 ***
vehicle_weight    1.245e-03  6.890e-04   1.807   0.0776 .  
spoiler_angle     6.877e-02  6.653e-02   1.034   0.3069    
ground_clearance  3.546e+00  5.412e-01   6.551 5.21e-08 ***
AWD              -3.411e+00  2.535e+00  -1.346   0.1852    
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 8.774 on 44 degrees of freedom
Multiple R-squared:  0.7149,	Adjusted R-squared:  0.6825 
F-statistic: 22.07 on 5 and 44 DF,  p-value: 5.35e-11

```

### Deliverable 2: Summary Statistics on Suspension Coils

```
> # Calculate statistics (mean, median, variance, std deviation of Susp Coil's PSI)
> Mean = mean(suspcoil_df$PSI)
> Median = median(suspcoil_df$PSI)
> Variance= var(suspcoil_df$PSI)
> SD = sd(suspcoil_df$PSI)
> # Calculate Total Summary Data Frame
> total_summary <- data.frame(Mean, Median, Variance, SD)
> # Summary by lot
> lot_summary <- suspcoil_df %>% group_by(Manufacturing_Lot) %>% summarize(Mean=mean(PSI), Median=median(PSI), Variance=var(PSI), SD=sd(PSI), .groups='keep')
```      
<img src="images/TotalSummary.png">                       <img src="images/LotSummary.png">

### Deliverable 3: T-Test on Suspension Coils
```
> # First Test on 'all lots'
> t.test((suspcoil_df$PSI), mu=1500)

	One Sample t-test

data:  (suspcoil_df$PSI)
t = -1.8931, df = 149, p-value = 0.06028
alternative hypothesis: true mean is not equal to 1500
95 percent confidence interval:
 1497.507 1500.053
sample estimates:
mean of x 
  1498.78 

> # Second Test on 'each lot'
> t.test(subset(suspcoil_df, Manufacturing_Lot == "Lot1")$PSI, mu=1500)

	One Sample t-test

data:  subset(suspcoil_df, Manufacturing_Lot == "Lot1")$PSI
t = 0, df = 49, p-value = 1
alternative hypothesis: true mean is not equal to 1500
95 percent confidence interval:
 1499.719 1500.281
sample estimates:
mean of x 
     1500 

> t.test(subset(suspcoil_df, Manufacturing_Lot == "Lot2")$PSI, mu=1500)

	One Sample t-test

data:  subset(suspcoil_df, Manufacturing_Lot == "Lot2")$PSI
t = 0.51745, df = 49, p-value = 0.6072
alternative hypothesis: true mean is not equal to 1500
95 percent confidence interval:
 1499.423 1500.977
sample estimates:
mean of x 
   1500.2 

> t.test(subset(suspcoil_df, Manufacturing_Lot == "Lot3")$PSI, mu=1500)

	One Sample t-test

data:  subset(suspcoil_df, Manufacturing_Lot == "Lot3")$PSI
t = -2.0916, df = 49, p-value = 0.04168
alternative hypothesis: true mean is not equal to 1500
95 percent confidence interval:
 1492.431 1499.849
sample estimates:
mean of x 
  1496.14 
```

## Summary

When looking at Deliverable 1, the major impacts on MPG are car weight, spoiler angle and AWD collectivly. This means the designers at MechaCar have to pay close attention to all three when making adjustments as all 3 impact the MPG, so just focusing on one without the other 2 may not improve the MPG and could increase it by making the other 2 worse.

When looking at Deliverables 2 and 3, they both show, with Lot 3, that the lower suspension coil PSI's, these have the biggest variance and fall outside of the range of coils that MechaCar should be using.  I would recommend to MechaCar that the suspension coils in Lot 3 be removed from their list of coils to choose from.

### Deliverable 4: Design a Study Comparing the MechaCar to the Competition

To look at MechaCar vs Competors, the company should invest into an ANOVA test

Additional analysis that would be beneficial would be:
  * The number of downtown gyms or buildings with gyms and changing rooms for commuters.
  * The analysis of the different months throughout the year and temperatures to see how the number of rides differed.

Thank you for your time, and let me know if you wish to see any additional data.

Jill Hughes
