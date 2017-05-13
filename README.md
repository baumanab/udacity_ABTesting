# A/B Testing to Determine an Effective Intervention to Decrease Early Udacity Course Cancellation

## Experiment Description

At the time of the experiment described herein, the Udacity course home pages have two options: "start free trial" and "access course materials."  Clicking "start free trial" prompts the user to enter their credit card information, subsequently enrolling them in a 14 day free trial of the course, after which they are automatically charged.  Users who click "access course materials" will be able to view course content but receive no coaching support, verified certificate, or project feedback.  

For this experiment Udacity tested a change wherein those users who clicked "start free trial" were asked how much time they were willing to devote to the course.  Users choosing 5 or more hours per week would be taken through the checkout process as usual.  For users indicating fewer than 5 hours per week a message would appear indicating the need for a greater time commitment to enable success and suggesting they might like to access the free content.  At this point the student would have the option to continue enrolling in the free trial or access the course materials for free.  
This screenshot shows the experiment:

![Experiment Screenshot](https://github.com/baumanab/udacity_ABTesting/Final Project- Experiment Screenshot.png)

The rationale for this change is that diverting students as a function of time to devote to study might improve the overall student expereince and the coaches' capacity to support students who are likely to complete the course, without significantly reducing the number of students who continue past the free trial.

## Experiment Design

The initial unit of diversion to the conrol and experiment groups is a unique cookie.  However, once a student enrolls in the free trial, they are tracked by user-id.  The same user-id can't enroll more than once.  Users who don't enroll are not tracked by user-id.  Note that the uniqueness of a cookie is determined per day.


### Metric Choice

_**Invariant Metrics:** number of cookies, number of clicks, click-through-probability_

_**Evaluation Metrics:** gross conversion, retention, net conversion_


#### Invariant Metrics

Invariant metrics were chosen due to their expected property of being....well, invariant.  One would expect similiar distribution into control and experiment for the following metrics.  If one were to find that this is not the case, it could be indicative of feeding mogwai after midnight.

**Number of Cookies:** The number of unique cookies to visit the course overview page.  This is the unit of diversion and even distribution amongst the control and experiment groups is expected.  It is therefore appropriate as an invariant metric.

**Number of Clicks:** The number of users (tracked as unique cookies at this stage) to click the free trial buttion. This is appropriate as an invariant metric but not an evaluation metrice.  Equal distribution amongst the experiment and control groups would be expected since at this point in the funnel the experience is the same for all users and therefore elements of the experiment would not be expected to impact clicking the "start free trial" button.    

**Click-through-probability:** Unique cookies to click the "start free trial" button per unique cookies to view the course overview page. Equal distribution amongst the experiment and control groups would be expected since at this point in the funnel the experience is the same for all users and therefore elements of the experiment would not be expected to impact clicking the "start free trial" button.    
 

#### Evaluation Metrics

Evaluation metrics were chosen since there is the possibility of different distributions between experiment and control groups as a function of the experiment.  Each evaluation metric is associated with a minimum difference (dmin) that must be observed for consideration in the decision to launch the experiment. The ultimate goal is to minimize student frustration and satisfaction and to most effectively use limited coaching resources.  Cancelling early may be one indication of frustration or low satisfaction and the more students enrolled in the course who do not make at least one payment, much less finish the course, the less coaching resources are being used effectively.  With this in mind, in order to consider launching the experiment either of the following must be observed:

- Increased retention (more students staying beyond the free trial in the experiment group)
- Decreased Gross Conversion coupled to increased Net Conversion (less students enrolling in the free trial but more students staying beyond the free trial)


**Gross Conversion:**  This is the number of user-ids to complete checkout and enroll in the free trial per unique cookie to click the "start free trial" button.  dmin = 0.01


**Retention:**  This is the number of user-ids to remain enrolled past the 14 day trial period, making at least one payment, per number of user-ids to complete checkout.  The practical minimum difference (dmin) = 0.01


**Net Conversion:** this is the number of user-ids to remain enrolled past the 14 day trial, making at least one payment, per the number of unique cookies to click the "start free trial" button.  dmin = 0.0075

#### Unused Metrics

**Number of user-ids:** The number of users to enroll in the free trial.  This is not a suitable invariant metric and while it could be used as an evalution metric, it is not ideal.  User-ids are tracked only after enrolling in the free trial and equal distribution between the control and experimental branches would not be expected.  User-id count could be used to evaluate how many enrollments stayed beyond the 14 day free trial boundary, but since it isn't normalized, I have elected not to use it.  


## Measuring Standard Deviation

**_Analytical Estimate of Standard Deviation_**

| Evaluation Metric | Standard Deviation |
|:-------------------:|:--------------------:|
| Gross Conversion  | .0202 |
| Retention         | .0549 |
| Net Conversion    | .0156 |

The analytical estimate of standard devation tends to be near the empirically determined standard deviation for those cases in which the unit of diversion is equal to the unit of analysis.  This is the case for Gross Conversion and Net Conversion, but not Retention.  If we do ultimately decide to use Retention, then we should calculate the empirical variability.


## Sizing

The following calcations are based on [baseline conversion data](data/baseline_vals.csv).

### Number of Samples vs. Power

My intial approach will not deploy the Bonferroni correction, that decision will be made based on my final choice of evaluation metrics and associated criteria. Pageviews required for each metric were calculated using an alpha value of 0.05 and beta value of 0.2.

**_Pageviews for Each Evaluation Metric to Achieve Target Statistical Power_**

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

If we divert 100% of traffic, given 40,000 page views per day, the experiment would take ~ 119 days.  If we eliminate retention, we are left with Gross Conversion and Net Conversion.  This reduces the number of required pageviews to 685,325, and an ~ 18 day experiment with 100% diversion and ~ 35 days given 50% diversion.  

A 119 day experiment with 100% diversion of traffic presents both a business risk (potential for: frustrated students, lower conversion and retention, and inefficient use of coaching resources) and an opportunity risk (performing other experiments).  However, in general, this is not a risky experiment as the change would not be expected to cause a precipitous drop in enrollment.  In terms of timing, an 18 day experiment is more reasonable, but % diversion may be scaled down depending on other experiments of interest to be performed concurrently.


## Experiment Analysis

The experimental data can be found in the following links:

- [experiment group](data/Final Project Results - Experiment.csv)
- [control group](data/Final Project Results - Control.csv)

### Sanity Checks

For invariant metrics we expect equal diversion into the experiment and control group.  We will test this at the 95% confidence interval.

| Metric | Expected Value | Observed Value | CI Lower Bound | CI Upper Bound | Result |
|:------:|:--------------:|:--------------:|:--------------:|:--------------:|:------:|
| Number of Cookies | 0.5000 | 0.5006 | 0.4988 | 0.5012 | Pass |
| Number of clicks on "start free trial" | 0.5000 | 0.5005 | 0.4959 | 0.5042 | Pass |
| Click-through-probability | 0.0821 | 0.0822 | 0.0812 | 0.0830 | Pass | 


### Result Analysis

95% Confidence interval for the difference between the experiment and control group for evaluation metrics.

| Metric | dmin | Observed Difference | CI Lower Bound | CI Upper Bound | Result |
|:------:|:--------------:|:--------------:|:--------------:|:--------------:|:------:|
| Gross Conversion | 0.01 | -0.0205 | -.0291 | -.0120 | Satistically and Practically Significant |
| Net Conversion | 0.0075 | -0.0048 | -0.0116 | 0.0019 | Neither Statistically nor Practically Significant |


### Sign Tests

| Metric | p-value for sign test | Statistically Significant @ alpha .05? |
|:------:|:--------------:|:--------------:|
| Gross Conversion | 0.0026 | Yes |
| Net Conversion | 0.6776 | No |

## Summary

An experiment was conducted in which potential Udacity students were diverted by cookie into two groups, experiment and control.  The experiment group was asked to input the amount of time they are willing to devote to study, after clicking a "start free trial button", whereas the control group was not.  Three invariant metrics (Number of Cookies, Number of clicks on "start free trial", and Click-Through-Probability) were chosen for purposes of validation and sanity checking while Gross Conversion (enrollment/cookie) and Net Conversion (payments/cookie) served as evaluation metrics.  The null hypothesis is that there is no difference in the evaluation metrics between the groups, futhermore, a practical signifcance threshold was set for each metric.  The requirement for launching the experiment is that the null hypothesis must be rejected for ALL evaluation metrics and that the difference between branches must meet or exceed the practical signficance threshold.  Because our acceptance criteria requires statiscally signifcant differences for ALL evaluation metrics, the use of the Bonferonni correction is not appropriate.  The Bonferonni correction is a method for controlling for type I errors (false positives) when using multiple metrics in which relevance of ANY of the metrics matches the hypothesis.  In this case the risk of type I errors increases as the number of metrics increases (signifcance by random chance).  In our case in which ALL metrics must be relevant to launch, the risk of type II errors (false negatives) increases as the number of metrics increases, so it stands to reason that controlling for false positives is not consistent with our acceptance criteria. 

Analysis revealed the expected equal distribution of cookies into the control and experimental groups, for the invariant metrics, at the 95% CI.  A difference in gross conversion was found to be statistically signficant at the 95% CI, and the null hypothesis was rejected.  Gross conversion also met the practical signficance threshold.  Net Conversion was found to be neither statistically nor practically signficant at the 95% CI.     


## Recommendation

This experiment was designed to determine whether filtering students as a function of study time commitment would improve the overall student experience and the coaches' capacity to support students who are likely to complete the course, without significantly reducing the number of students who continue past the free trial. A statistically and practically signficant decrease in Gross Conversion was observed but with no significant differences in Net Conversion. This translates to a decrease in enrollment not coupled to an increase in students staying for the requisite 14 days to trigger payment.  Considering this, my recomendation is not to launch, but rather to pursue other experiments.


## Follow-Up Experiment

The construct of student frustration could be assigned an operational definition of "cancel early," where a convenient definition and measure of early cancellation is prior to the end of the 14 day trial period in which payment is triggered.  An early cancellation is not necessarily indicative of frustration but could be from other causes, such as a course not being aligned to the students needs or expectations in terms of content. For preventng early cancellation there are two primary logical timepoint opportunities for intervention, (1) pre-enrollment, and (2) post-enrollment but pre-payment. 

The first opportunity for intervention was explored above wherein a poll regarding time commitment was used as to filter out students likely to become frustrated.  This filter focused only on time commitment to the class and did not address other reasons why a student might become frustrated and cancel early. Even if the student was sincere in their response and dilligent in their study, they may become frustrated if they don't have the suggested pre-requisite skills and experience.  That is, their committed time may not be enough if they don't come in with the pre-requisite skill set.  Adding a checklist of pre-requisite skills to the popup regarding time commitment may be informative.  This experiment would leverage the infrastrucure and data pipeline of the original experiment and be set up in the same way as the original, including the unit of diversion.  The only difference would be the information in the form.  If the student's answer meets the time and pre-requisite requirements (radiobox checklist) they are directed to enroll in the free trial, otherwise they are encouraged to access the free version.  This experiment would be low cost in terms of resources and may increase the selectivity of the pre-enrollment filter.  A succesful experiment would be one in which there is a signficant decrease in Gross Conversion coupled to a significant increase in Net Conversion.

A variety of approaches could be used to intervene post-enrollment but pre-payment and could be deployed concurrently with pre-enrollment intervention.  An ideal approach would be one which minimizes the use of additional coaching resources to best meet the original intent of the intervention.  An effective approach may be to employ peer coaching/guidance by means of team formation.  If a student has a team of other students which they could consult, discuss coursework and frustrations with, and be accountable to, they may be more likely to stick out the growing pains and stay for the long term.  The experiment would function in the following manner.

**Setup:** Upon enrollment students will either be randomly assigned to a control group in which they are not funnelled into a team, or an experiment group in which they are.

**Null Hypothesis:** Participation in a team will not increase the number of students enrolled beyond the 14 day free trial period by a significant amount.

**Unit of Diversion:** The unit of diversion will be user-id as the change takes place after a student creates an account and enrolls in a course.

**Invariant Metrics:**  The invariant metric will be user-id, since an equal distribution between experiment and control would be expected as a property of the setup.

**Evaluation Metrics:** The evaluation metric willl be Retention.  A statistically and practically significant increase in Retention would indicate that the change is succesful.

If a statistically and practically signifcant positive change in Retention is observed, assuming an acceptable impact on overall Udacity resources (setting up and maintaining teams will require resource use), the experiment will be launched.

