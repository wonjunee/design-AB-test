#Final Project of Design an A/B Test Course
##Udacity Data Analyst Nanodegree Project 7

###Experiment Design
####Metric Choice
*Invariant metrics*: Number of Cookies and Number of clicks
*Evaluation metrics*: Gross Conversion and Net Conversion

- **Number of cookies**: I chose it as an invariant metric because it will be consistent throughout the experiment. It is calculated before the user sees the pop-up messsage, so the number will be similar in experimenta and control groups.
- **Number of user-ids**: I didn't choose this as an invariant metric because this value is based on whether the user decide to enroll after the pop-up message. So the number of user-ids might be different between the experiment and control groups. It is not a good evaluation metric as well because the the values can be different in each day. 
- **Number of clicks**: Similar to the number of cookies, this value is independent from the experiment, so the number will be similar in experiment and control groups.
- **Click-through-probability**: A good invariant metric because it is independent from the experiment. However, I didn't choose this metric as an invariant metric for the test because this value seems to be redundant to the number of cookies and the number of clicks.
- **Gross Conversion**: This is a good evaluation metric because it is dependent on the experiment (This explains why it cannot be a good invariant metric). We expect that the value will be lower in the experiment group because some people who are not able to commit more than 5 hours per week won't enroll the classes. In the control group, users won't see any pop-up message so they will enroll without any consideration of the number of hours they can commit per week.
- **Retention**: This is a good evaluation metric because it is dependent on the experiment (This explains why it cannot be a good invariant metric). We expect this value will be higher in the experiment group because majority of the group are those who can commit 5 hours per week and these people are more likely to make the first payments for the classes. However, I didn't choose this as an evaluation metric for the test because it is redundant to Gross conversion and Net conversion; this value can be calculated using Retention and Net Conversion.
- **Net Conversion**: This is a good evaluation metric because it is dependent on the experiment (This explains why it cannot be a good invariant metric). We expect this value will be higher in the experiment group because majority of the group are those who can commit 5 hours per week and these people are more likely to make the first payments for the classes (The assumption and expectation are similar to Retention).


###Measuring Standard Deviation
List the standard deviation of each of your evaluation metrics. (These should be the answers from the "Calculating standard deviation" quiz.)

```
Standard Deviation of Gross Conversion: **0.0202**
Standard Deviation of Net Conversion: **0.0156**
```

**Gross Conversion** and **Net Conversion** use **the number of cookies** as their denominators. This is a unit of diversion and it is same as a unit of analysis; thus, analytical estimate is comparable to the empirical variability.

###Sizing
####Number of Samples vs. Power
####Will you use the Bonferroni correction in your analysis phase?####

No. The metrics are not independent from the others so Bonferroni correction will be convservative to use for this test.

####The number of pageviews I need to power the experiment: ####

I calculated the pageviews from the calculator from http://www.evanmiller.org/ab-testing/sample-size.html with a baseline conversion rate = 10.931% (Net Conversion rate) and minimum detectable effect = 0.75%. As a result, I got 27,413. However, this number is the size we need to power the experiment for net conversion rate. To find the pageviews we need, we need to take account that among 40,000 people viewing the page, only 3,200 people clicks the “Start free trial”. Thus, 27,413 * 40,000 / 3,200 = 342,662.5 pageviews. There are two groups, control and experiment, so we need to double this value. The toal pageviews are 685,325.

####Duration vs. Exposure

The fraction of traffic I would divert to this experiment is .5 (from 0 to 1). Given this fraction, I need 685,325 / 40,000 = 34.__ -> 35 days to run the experiment.

*Give your reasoning for the fraction you chose to divert. How risky do you think this experiment would be for Udacity?*

Only 50% will be affected and the change due to the experiment is small, so it won't cause too much trouble in the overall business. Furthermore, 35 days of the experiment is the enough amount of time to really know if the change really affects the users' decisions to take the course.

###Experiment Analysis
####Sanity Checks

```
Number of cookies: Lower bound = 0.4988, Upper bound = 0.5012, Observed = 0.5006: Passed
SE = sqrt( p * (1-p) * (1/Number of Control Pageviews + 1/Number of Experiment Pageviews)
= sqrt( 0.5 * 0.5 * (1/344660 + 1/345543) = 0.0006
Margin of error = 0.0006 * 1.96 = 0.0012
Lower bound = 0.5 – 0.0012 = 0.4988, Upper bound = 0.5 + 0.0012 = 0.5012
Observed = 345543 / (345543 + 344660) = 0.5006

Number of clicks: Lower bound = 0.4959, Upper bound = 0.5041, Observed = 0.5005: Passed
SE = sqrt( p * (1-p) * (1/Number of Control Clicks + 1/Number of Experiment Clicks)
= sqrt( 0.5 * 0.5 * (1/28325 + 1/28378) = 0.0021
Margin of error = 0.0021 * 1.96 = 0.0041
Lower bound = 0.5 – 0.0041 = 0.4959, Upper bound = 0.5 + 0.0041 = 0.5041
Observed = 28378 / (28378 + 28325) = 0.5005
```

###Result Analysis
####Effect Size Tests
For each of your evaluation metrics, give a 95% confidence interval around the difference between the experiment and control groups. Indicate whether each metric is statistically and practically significant. (These should be the answers from the "Effect Size Tests" quiz.)

```
Gross Conversion: Lower bound = -0.0291, Upper bound = -0.0120
	Statistical Significance: Yes, Practical Significance: Yes

Net Conversion: Lower bound = -0.0116, Upper bound = 0.0019
	Statistical Significance: No, Practical Significance: No
```

####Sign Tests

```
Gross Conversion: p-value = 0.0026, Statistical Significance = Yes
Net Conversion: p-value = 0.6776, Statistical Significance = No
```

####Summary
*State whether you used the Bonferroni correction, and explain why or why not. If there are any discrepancies between the effect size hypothesis tests and the sign tests, describe the discrepancy and why you think it arose.*

I didn’t use Bonferroni correction because the metrics are not independent; thus, Bonferroni correction is too conservative for the test.

We would have used Bonferroni correction if we expected not all of the metrics we chose showed siginificant changes after the test. In this case, type I error would be very high and Bonferroni correction can be used to reduce this type of risk. However, in this test, we expect all of the metrics show significant changes; thus Type II error will be high, and Bonferroni is not helpful in this case.

Both effect size tests and sign tests result in having statistical significance in **Gross Conversion** but no statistical significance in **Net Conversion**. This means that although the experiment affects the users' decision to enroll the online courses but it didn't affect much to the enrolled users to pay the courses.

####Recommendation
Make a recommendation and briefly describe your reasoning.

I would recommend not to launch the experiment. The result of Gross Conversion showed statistically and practically significant changes after the pop-up message. This means that only those who are likely to commit more than 5 hours per week enroll the classes and they are more likely to make the first payments; this will reduce the costs of having those who are only taking free courses without making any payment. However, Net Conversion don't show statistically or practiclly significant changes after the test. This means that showing pop-up message to users before they enroll the classes don't affect the number of users who make their first payments after the free trial. Furthermore, the confidence interval of the net conversion includes the negative of the practical significance boundary which suggests the risk of hurting the Udacity's businiess.

###Follow-Up Experiment
*Give a high-level description of the follow up experiment you would run, what your hypothesis would be, what metrics you would want to measure, what your unit of diversion would be, and your reasoning for these choices.*


