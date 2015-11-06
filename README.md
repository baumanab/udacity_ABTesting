# Udacity AB Testing Final Project

## Experiment Description

At the time of the experiment described herein, the Udacity course home pages have two options: "start free trial" and "access course materials."  Clicking "start free trial" prompts the user to enter their credit card information, subsequently enrolling them in a 14 day free trial of the course, after which they are automatically charged.  Users who click "access course materials" will be able to view course content but receive no coaching support, verified certificate, or project feedback.  

For this experiment Udacity tested a change wherein those users who clicked "start free trial" were asked how much time they were willing to devote to the course.  Users choosing 5 or more hours per week would be taken through the checkout process as usual.  For users indicating fewer than 5 hours per week a message would appear indicating the need for a greater time requirement to be succesful in the course and suggesting they might like to access the free content.  At this point the student would hae the option to continue enrolling in the free trial or access the course materials for free.  
This screenshot shows the experiment:

![Experiment Screenshot](Final Project- Experiment Screenshot.png)

The rationale for this change is that diverting students as a function of time to devote to study might improve the overall student expereince and the coaches' capacity to support students who are likely to complete the course, without significantly reducing the number of students who continue past the free trial.

## Experiment Design
The initial unit of diversion to the conrol and experiments groups is a unique cookie.  However, once a student enroll in the free trial they are tracked by user-id.  The same user-id can't enroll more than once.  Users who don't enroll are not tracked by user-id.  Note that the uniqueness of a cookie is determined per day.

### Metric Choice
_**Invariant Metrics:** number of cookies, number of clicks_

_**Evaluation Metrics:** gross conversion, retention, net conversion_


#### Invariant Metrics

Invariant metrics were chosen due to their expected property of being....well, invariant.  One would expect similiar distribution into control and experiment for the following metrics.  If one were to find that this is not the case, it could be indicative of feeding mogwai after midnight.

**Number of Cookies:** The number of unique cookies to visit the course overview page.  This is the unit of diversion and even distribution amongst the control and experiment groups is expected, and it is therefore appropriate as an invariant metric.

**Number of Clicks:** The number of users (tracked as unique cookies at this stage) to click the free trial buttion. This is appropriate as an invariant metric but not an evaluation metrice.  Equal distribution amongst the experiment and control groups would be expected since at this point in the funnell the experience is the same for all users and therefore elements of the experiment would not be expected to impact clicking the "start free trial" button.    

#### Evaluation Metrics

Evaluation metrics were chosen since there is the possibility of different distributions between experiment and control groups as a function of the experiment.  Each evaluation metric is associated with a minimum difference (dmin) that must be observed for consideration in the decision to launch the experiment.  There are constructs and elements at play that would be impacted upon making the change being evaluated in the experiment, not measured by the experiemnt (e.g. student frustration/satisfaction, effective use of limited coaching resources).  Therefore the minium barrier to consider making change is meeting or exceeding dmin for one evaluation metric.  If this threshold is reached followup experiments my be considred prior to making any changes.  

**Gross Conversion:**  This is the number of user-ids to complete checkout and enroll in the free trial per unique cookie to click the "start free trial" button.  dmin = 0.01


**Retention:**  This is the number of user-ids to remain enrolled past the 14 day trial period, making at least one payment, per number of use-ids to complete checkout.  The practical minimum difference (dmin) = 0.01


**Net Conversion:** this is the number of user-ids to remain enrolled past the 14 day trial, making at least one payment, per the number of unique cookies to click the "start free trial" button.  dmin = 0.0075



## Measuring Standard Deviation

**_Analytical Estimate of Standard Deviation_**

| Evaluation Metric | Standard Deviation |
|:-------------------:|:--------------------:|
| Gross Conversion  | .0202 |
| Retention         | .0549 |
| Net Conversion    | .0156 |

The analtytical estimate of standard devation tends to be near the empircally determined standard deviation for those cases in which the unit of diversion is equal to the unit of analysis.  This is the case for Gross Conversion and Net Conversion, but not Retention.  If we do ultimately decide to use Retention, then we should calculate the empericaly variability.


## Sizing

The following calcations are based on [baseline conversion data](data/baseline_vals.csv).


### Number of Samples vs. Power
To reduce the complexity of this analysis my intial approach will not deploy the Bonferroni correction.  Pageviews required for each metric were calculated using an alpha value of 0.05 and beta value of 0.2.

**_Pageviews for Each Evaluation Metric to Achieve Target Statistial Power_**

#### Gross Conversion

- Baseline Conversion: 20.625%
- Minimum Detectable Effect: 1%
- alpha: 5%
- beta: 20%
- 1 - beta: 80%
- sample size = 25,835 enrollments/group
- Number of groups = 2 (experiment and control)
- total sample size =  51,670 enrollments
- clicks/pageview: 3200/40000 = .08 clicks/pageview
- pageviews = 645,875



#### Retention

- Baseline Conversion: 53%
- Minimum Detectable Effect: 1%
- alpha: 5%
- beta: 20%
- 1 - beta: 80%
- sample size = 39,155 enrollments/group
- Number of groups = 2 (experiment and control)
- total sample size = 78,230 enrollments
- enrollments/pageview: 660/40000 = .0165 enrollments/pageview
- pageviews = 78,230/.0165 = 4,741,212

#### Net Conversion

- Baseline Conversion: 10.9313%
- Minimum Detectable Effect: .75%
- alpha: 5%
- beta: 20%
- 1 - beta: 80%
- sample size = 27,413 enrollments/group
- Number of groups = 2 (experiment and control)
- total sample size = 54,826
- clicks/pageview: 3200/40000 = .08 clicks/pageview
- pageviews = 685,325

_Pageviews Required:  4,741,212_


### Duration vs. Exposure
If we divert 100% off traffic, given 40,000 page views per day, the experiment would take ~ 119 days.  If we eliminate retention, we are left with Gross Conversion and Net Conversion.  This reduces the number of required pageviews to 685,325, and an ~ 18 day experiment with 100% diversion and ~ 35 days give 50% diversion.  

A 119 day experiment with 100% diversion of traffic presents both a business risk (potential for: frustrated students, lower conversion and retention, and inefficient use of coaching resources) and an opportunity risk (performing other experiments).  An 18 day experiment is more reasonable, but % diversion may be scaled down depending on other experiments of interest to be performed concurrently.


## Experiment Analysis
Sanity Checks
For each of your invariant metrics, give the 95% confidence interval for the value you expect to observe, the actual observed value, and whether the metric passes your sanity check. (These should be the answers from the "Sanity Checks" quiz.)


For any sanity check that did not pass, explain your best guess as to what went wrong based on the day-by-day data. Do not proceed to the rest of the analysis unless all sanity checks pass.


## Result Analysis
Effect Size Tests
For each of your evaluation metrics, give a 95% confidence interval around the difference between the experiment and control groups. Indicate whether each metric is statistically and practically significant. (These should be the answers from the "Effect Size Tests" quiz.)


## Sign Tests
For each of your evaluation metrics, do a sign test using the day-by-day data, and report the p-value of the sign test and whether the result is statistically significant. (These should be the answers from the "Sign Tests" quiz.)


## Summary
State whether you used the Bonferroni correction, and explain why or why not. If there are any discrepancies between the effect size hypothesis tests and the sign tests, describe the discrepancy and why you think it arose.


## Recommendation
Make a recommendation and briefly describe your reasoning.


## Follow-Up Experiment
Give a high-level description of the follow up experiment you would run, what your hypothesis would be, what metrics you would want to measure, what your unit of diversion would be, and your reasoning for these choices.
