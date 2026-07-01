# utkarshshahare_2511900___part2_kpi_experiment

# Part 2: KPI Framework, Business Experiment Analysis & Decision Recommendation

# 1. Business Problem

## Business Context

The organization recently launched a redesigned onboarding and activation experience to encourage a higher percentage of new users to upgrade from free accounts to paid subscriptions.

### Business Decision

Before making this onboarding flow the default experience for every customer, the company must determine whether it produces a measurable improvement over the existing onboarding process. The objective is to confirm that the new experience delivers enough business value to justify a full rollout.

### Stakeholders

The outcome of this experiment affects multiple departments, including:

- Product
- Marketing
- Customer Success
- Revenue Operations
- Executive Leadership

The decision will influence customer acquisition strategy, user experience, operational efficiency, and subscription revenue.

### Primary Success Metric

The most important metric for this experiment is **Paid Conversion Rate**, which represents the proportion of users who successfully upgrade to a paid subscription.

Additional improvements in onboarding completion and user engagement are also considered positive indicators of success.

### Potential Risks

While improving conversion is the primary objective, the company also needs to monitor for any negative side effects, including:

- Increase in customer support requests
- Higher refund frequency
- Longer time required to convert
- Declining customer satisfaction
- Uneven performance across different customer segments

### Evidence Needed

A recommendation should only be made if the experiment demonstrates a statistically meaningful improvement in the Treatment group while maintaining acceptable performance on important guardrail metrics such as:

- Refund Rate
- Support Ticket Rate
- Engagement Score
- Days to Convert

---

# 2. North Star Metric

## Selected Metric

### Paid Conversion Rate

**Formula**

> Paid Conversion Rate = Number of Paid Users ÷ Total Users

## Why This Metric Was Chosen

The purpose of the onboarding campaign is to convert a larger percentage of users into paying subscribers. Since the business follows a subscription-based revenue model, improvements in Paid Conversion Rate have a direct impact on company revenue.

Because of this direct relationship with business growth, Paid Conversion Rate serves as the primary indicator of experiment success.

## Why Other Metrics Are Secondary

Several additional metrics help explain customer behavior throughout the onboarding journey, but they do not directly measure business success.

Examples include:

- Landing Page Visit Rate — measures initial user interest.
- Trial Start Rate — indicates activation.
- Onboarding Completion Rate — reflects onboarding effectiveness.
- Engagement Score — measures interaction with the product.

Although these metrics provide valuable context, they ultimately support the primary objective of increasing paid subscriptions.

## Business Impact

A higher Paid Conversion Rate means more customers are purchasing subscriptions, leading to:

- Increased recurring revenue
- Better return on customer acquisition costs
- Sustainable long-term business growth

## Risks of Focusing Only on Conversion

Optimizing only for conversion can create unintended consequences.

For example, users may subscribe initially but later experience frustration, resulting in:

- More refund requests
- Higher support workload
- Lower long-term retention

For this reason, guardrail metrics such as Refund Rate, Support Ticket Rate, Engagement Score, and Days to Convert should always be reviewed alongside Paid Conversion Rate.

---

# 3. Data Cleaning and Preparation

Before evaluating experiment performance, several quality checks were performed to verify that the dataset was reliable for A/B testing.

## Missing Value Assessment

Most variables were complete with very few missing observations.

Small numbers of missing values were identified in:

- `device_type`
- `traffic_source`
- `engagement_score`

A much larger number of missing values appeared in `days_to_convert`. This was expected because users who never upgraded to a paid subscription do not have a conversion date.

No corrective action was necessary.

| Column | Missing Values |
|--------------------------|---------------:|
| user_id | 0 |
| signup_date | 0 |
| experiment_group | 0 |
| region | 0 |
| device_type | 18 |
| traffic_source | 24 |
| plan_type | 0 |
| visited_landing_page | 0 |
| started_trial | 0 |
| completed_onboarding | 0 |
| converted_to_paid | 0 |
| revenue_30d | 0 |
| support_tickets_30d | 0 |
| refund_requested | 0 |
| days_to_convert | 1335 |
| engagement_score | 14 |

---

## Experiment Group Validation

The dataset consisted of **1,408 users**.

The assignment between Control and Treatment groups was well balanced.

| Group | Users |
|---------|------:|
| Control | 693 |
| Treatment | 715 |
| **Total** | **1408** |

The nearly equal distribution supports a fair comparison between both experiment groups.

---

## Duplicate User Review

Duplicate records were checked using the `user_id` field.

The analysis identified:

| Validation | Result |
|--------------------------|--------------------------------|
| Duplicate User IDs | 8 |
| Action | Reviewed and flagged before analysis |

Since the duplicates represented only a very small portion of the dataset, they were retained because they did not materially influence the experiment results.

---

## Binary Variable Validation

Binary fields were verified to ensure they contained only valid values (`0` and `1`).

| Column | Valid Values | Status |
|----------------------|--------------|--------|
| visited_landing_page | 0,1 | Pass |
| started_trial | 0,1 | Pass |
| completed_onboarding | 0,1 | Pass |
| converted_to_paid | 0,1 | Pass |
| refund_requested | 0,1 | Pass |

No invalid values were found.

---

## Revenue Outlier Review

Revenue values were examined using the Interquartile Range (IQR) method.

| Metric | Value |
|-------------------------------|---------:|
| Minimum Revenue | 0.00 |
| Maximum Revenue | 8610.72 |
| Average Revenue | 52.83 |
| Records Above Upper Bound | 72 |

The revenue distribution was heavily skewed because many users generated no revenue.

Since both Q1 and Q3 were zero, the calculated IQR was also zero, causing 72 observations to be flagged as potential outliers.

These records were retained because they most likely represent legitimate high-value customers. Removing them would underestimate the experiment's true revenue impact.

---

## Segment Distribution Check

The distribution of important customer segments was compared between Control and Treatment groups to ensure that the experiment groups were reasonably similar before measuring outcomes.

### Region

| Region | Control | Treatment |
|---------|--------:|----------:|
| East | 158 | 172 |
| North | 203 | 180 |
| South | 184 | 184 |
| West | 148 | 179 |

### Device Type

| Device | Control | Treatment |
|----------|--------:|----------:|
| Desktop | 200 | 214 |
| Mobile | 428 | 436 |
| Tablet | 56 | 56 |
| Missing | 9 | 9 |

### Traffic Source

| Source | Control | Treatment |
|----------------|--------:|----------:|
| Email | 74 | 56 |
| Organic | 246 | 241 |
| Paid Search | 156 | 176 |
| Referral | 81 | 91 |
| Social | 130 | 133 |
| Missing | 6 | 18 |

### Plan Type

| Plan | Control | Treatment |
|-----------|--------:|----------:|
| Basic | 223 | 235 |
| Free | 361 | 368 |
| Premium | 109 | 112 |

### Summary

Overall, the Control and Treatment groups were well balanced across Region, Device Type, Traffic Source, and Plan Type.

Although small differences existed—such as slightly more users from the West region and Paid Search in the Treatment group—the distributions remained sufficiently similar to support a valid comparison.

Based on these validation checks, the dataset was considered suitable for A/B testing.

---

# 4. Experiment Summary

## Analysis Strategy

The experiment compared user behavior between:

- **Control Group** — Existing onboarding process
- **Treatment Group** — New onboarding experience

Performance was evaluated using the following KPIs:

- User Count
- Landing Page Visit Rate
- Trial Start Rate
- Onboarding Completion Rate
- Paid Conversion Rate
- Average Revenue Per User (ARPU)
- Average Revenue Per Converted User (ARPCU)
- Refund Rate
- Support Ticket Rate
- Average Engagement Score
- Average Days to Convert

The objective was to determine whether the redesigned onboarding flow improved customer activation, engagement, conversion, and revenue.

Beyond the overall comparison, Paid Conversion Rate was also analyzed across multiple customer segments, including Region, Device Type, and Traffic Source. This helped identify whether the treatment effect remained consistent across different groups.

Overall, the Treatment group performed better on several key business metrics, including onboarding completion, engagement, paid conversion, and revenue per user.

However, increases in Refund Rate and Support Ticket Rate were identified as potential guardrail concerns. These metrics should be considered carefully before deciding on a full-scale rollout of the new onboarding experience.
