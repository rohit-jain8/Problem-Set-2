
# ECON 3916 — Problem Set 2: The Survey That Wasn't Random

## Project Overview

This project examines how nonresponse bias can lead to misleading conclusions about customer satisfaction after a product onboarding redesign. I simulated a user population with known satisfaction scores, created a survey response process where more satisfied users were more likely to respond, and compared the survey results with the true population values.

The goal was to understand the difference between statistical uncertainty and systematic bias, and why a large sample or statistically significant result does not necessarily mean an estimate is accurate.

## Methods

The analysis is organized into four phases:

1. **Population simulation:** Simulated 50,000 users before and after the redesign, with satisfaction influenced by engagement, support-ticket history, and random variation. I then modeled survey participation as depending partly on satisfaction.
2. **Bootstrap confidence intervals:** Constructed percentile intervals manually and compared them with SciPy's percentile and bias-corrected and accelerated (BCa) intervals. I also used repeated simulations to evaluate confidence interval coverage under random sampling versus biased survey responses.
3. **Hypothesis testing and power:** Applied Welch's two-sample t-test, calculated Cohen's d, and used power analysis to examine sample-size requirements. I also investigated why increasing the number of biased responses reduces variability without removing bias.
4. **Inverse-probability weighting (IPW):** Used logistic regression to estimate response probabilities from engagement and support-ticket history, then weighted respondents by the inverse of their predicted response probabilities. A revised bootstrap refitted the response model and recalculated weights in each resample.

## Key Results

| Measure | Result |
| --- | ---: |
| True mean satisfaction before redesign | 3.2901 |
| True mean satisfaction after redesign | 3.3860 |
| True improvement | +0.0959 |
| Observed survey mean before redesign | 3.5999 |
| Observed survey mean after redesign | 4.3041 |
| Apparent improvement among respondents | +0.7041 |
| Post-redesign survey response rate | 15.19% |
| Observed post-redesign mean, 95% bootstrap percentile interval | [4.2892, 4.3192] |
| Welch's t-test | t = 65.166; p < 0.001 |
| Required sample size for 80% power to detect a 0.10-point difference | 1,236 per group |
| IPW-adjusted post-redesign mean | 4.1432 |
| Revised IPW bootstrap 95% interval | [4.1224, 4.1623] |

When drawing random samples from the simulated full population, the bootstrap percentile interval covered the true mean in 96% of repeated trials, and the BCa interval did so in 93%. When sampling from the biased respondent pool, both methods had 0% coverage in the simulation.

## Main Takeaway

The survey suggested an improvement of approximately **0.70 satisfaction points**, while the true simulated population improvement was only about **0.10 points**. This gap occurred because more satisfied users were more likely to complete the survey, especially after the redesign.

Although the observed difference was statistically significant, it did not establish that the redesign caused a population-wide improvement of that size. Increasing the number of responses made the estimates more precise but did not remove the selection bias.

Inverse-probability weighting reduced the post-redesign bias but did not eliminate it. The weighted estimate was **4.1432**, compared with the true population mean of **3.3860**. This is because response probability depended directly on satisfaction even after controlling for the available covariates, violating the conditional independence assumption needed for this weighting approach.

A better data-collection strategy would randomly select users to receive the survey and follow up with nonrespondents, rather than relying solely on voluntary responses. Random selection alone would not eliminate nonresponse bias, so improving and evaluating response rates would remain important.

## Tools and Libraries

- Python and Google Colab
- NumPy and pandas for simulation and data manipulation
- SciPy for bootstrap confidence intervals and hypothesis testing
- statsmodels for statistical power analysis
- scikit-learn for logistic regression and inverse-probability weighting
- Matplotlib for visualization

## Project Files

- `Econ_3916_PS2.ipynb` — Notebook containing the simulation, statistical analysis, interpretations, and AI transcript.
- `README.md` — Overview of the project, methods, findings, and limitations.

## Reproducing the Analysis

Open `Econ_3916_PS2.ipynb` in Google Colab and run the cells from top to bottom. The notebook uses fixed random seeds for reproducibility. The simulation generates its own data, so no external dataset is required.

---

**Course:** ECON 3916 — Applied Data Analytics  
**Assignment:** Problem Set 2 — The Survey That Wasn't Random
