# Strategic Pricing Analysis: Minimizing Risk In Price Elasticity Testing

## Executive Summary

This project evaluates the financial viability and operational risk of increasing a product's price point from $19.99 to $24.99.

Unlike a standard A/B test which seeks to find a "winner," this analysis employs Non-Inferiority Testing. The goal was to prove that the higher price point would not cause the conversion rate (CVR) to drop below a critical 8% break-even floor, thereby ensuring the revenue lift justifies the loss in customer volume.

__Key Outcome:__ The test passed the non-inferiority safety check ($p=0.0205$), projecting a 9.5% monthly revenue lift (+$1,901 per 10k visitors), despite a narrow statistical safety margin.


## Experimental Design

* __Methodology:__ Monadic A/B Test (Independent groups).

* __Primary Metric:__ Conversion Rate (CVR).

* __Counter-Metric:__ Revenue Per Visitor (RPV).

* __The "Safety" Hypothesis:__

   * __Null ($H_0$):__ Conversion will drop below the 8% absolute break-even floor.

   * __Alternative ($H_a$):__ Conversion will stay above the 8% floor (Non-Inferiority).

* __Sample Size:__ $N=5,000$ per group (exceeding the minimum power requirement of 2,524 to increase precision).


## Statistical Rigor & Python Implementation


The analysis was performed using statsmodels and scipy to ensure mathematical validity.

* __Non-Inferiority Z-Test:__ Validated that the treatment CVR is not worse than the control by more than the 2% allowable margin.

* __Confidence Interval Validation:__ Calculated the 95% CI for the difference in proportions to identify the "worst-case" scenario.

* __Sample Ratio Mismatch (SRM) Check:__ Performed a Chi-Square test on traffic distribution to ensure no randomization bias ($p=1.0$).

```

# Core logic: Testing for Non-Inferiority
z_stat, p_val = proportions_ztest(
            [conv_treat, conv_control],
            [n_treat, n_control],
            value=-0.02, # 2% Allowable Absolute Drop
            alternative='larger'
) 

```

## Visualizations & Interpretation

__1. The "Safety Margin" Analysis__

   Here, we see the comparison between the observed conversion rates against our 8.0% Break-even Floor.

   __Insight:__ While the Treatment group (blue) shows a drop in conversion compared to the Control (gray), the 95% Confidence Interval error bars remain above the red dashed "danger zone." This provides the statistical "green light" required to move forward with the rollout despite the volume loss.

   __Business Value:__ This visually confirms that even in a "worst-case" scenario, the price increase remains profitable.

<img width="675" height="411" alt="image" src="https://github.com/user-attachments/assets/d14c1237-15f5-476f-b92a-53c9d54a42ef" />

__2. Revenue Per Visitor (RPV) Lift__
   
   This illustrates the trade-off between volume and value.
   
   __Insight:__ We are trading a 12.4% drop in customer volume for a 10.05% increase in value per visitor.
   
   __Result:__ The higher price point generates an average of $2.19 per visitor vs. the baseline of $1.99.

<img width="674" height="405" alt="image" src="https://github.com/user-attachments/assets/3b9f5643-f610-46c8-816c-21cba5fda8b8" />

__3. The Volume vs. Value Trade-off__
   
   Here, we demonstrate the core business challenge of price elasticity.
   
   __Volume (Blue):__ Shows a 12.4% decrease in total conversions. We are successfully filtering out price-sensitive customers.
   
   __Value (Green):__ Despite fewer sales, total revenue increased by 9.5%.
   
   __Strategic Insight:__ This confirms the product has relatively low price elasticity, allowing for higher margins that more than compensate for the lower sales volume.

   <img width="937" height="538" alt="image" src="https://github.com/user-attachments/assets/bf7ea097-aa49-4508-98ef-3a736bca528a" />

## Key Insights for Stakeholders

__1. The "Safety" Margin__

   The statistical analysis revealed a Lower Bound 95% CI of -0.0195.

   * __Critical Insight:__ While the test passed, the conversion rate could realistically drop to 8.05%.

   * __The Risk:__ Since our break-even point is 8.00%, we are operating with a safety margin of < 0.1%. This highlights the need for a cautious rollout rather than an immediate 100% switch.

__2. Financial Impact__
   
   | Metric | Control ($19.99) | Treatment ($24.99) | Delta |
   | ------- | ------- | ------- | ------- |
   | Conversion Rate | 10.0% | 8.76% | -12.4% |
   | Revenue Per Visitor | $1.99 | $2.19 | +10.05% |
   | Projected Lift | -- | -- | +$1,901 / 10k users |

## Final Recommendation: Guarded Rollout

Based on the thin safety margin, I recommend a Guarded Rollout rather than a full 100% launch:

1. Phased Deployment: Launch to 75% of traffic to maintain a control group "safety net" for the first 30 days.

2. LTV Monitoring: Analyze if the 12.4% lost customer volume represents high-value long-term users or one-time shoppers.

3. Automated Kill-Switch: Implement a real-time monitor to trigger an automatic rollback if CVR dips below the 8.0% threshold.

## Project Structure

* ```strategic_pricing_analysis.ipynb```: Full Python implementation and statistical visualizations.

* ```data/```: Synthetic dataset representing the 10,000 test participants.

* ```requirements.txt```: Necessary libraries (pandas, numpy, statsmodels, scipy).

