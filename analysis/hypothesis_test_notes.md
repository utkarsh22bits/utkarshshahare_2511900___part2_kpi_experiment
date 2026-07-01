# Hypothesis Test Notes

## Task 6 — Framing the Hypothesis

**Metric being tested:** Paid conversion rate (`converted_to_paid` / total users per group)

**Reason for choosing this metric:** This is the North Star metric for the
experiment (see README.md for full reasoning). Leadership's actual decision —
whether to roll the new onboarding experience out to all users — depends on
whether it moves people to a paying subscription, not just whether it moves
upstream engagement metrics like landing page visits.

**Null hypothesis (H₀):** There is no difference in paid conversion rate between
the Control group (existing onboarding) and the Treatment group (new onboarding).
p_treatment = p_control

**Alternate hypothesis (H₁):** The Treatment group has a higher paid conversion
rate than the Control group. p_treatment > p_control

**Test type:** One-tailed (we are specifically testing whether the new experience
improves conversion, not just whether it differs in either direction — the
business question is "should we launch this," which only matters if treatment is
*better*).

**Significance level:** α = 0.05

**Interpretation logic:** If the resulting p-value is below 0.05, we reject the
null hypothesis and conclude the Treatment group's higher conversion rate is
unlikely to be due to chance alone. If p ≥ 0.05, we fail to reject the null and
treat the observed difference as not statistically distinguishable from random
variation.

## Task 7 — Test Execution

**Test used:** Two-proportion z-test (comparing the proportion of converted
users in Control vs. Treatment), run in Python using `statsmodels`.

### Summary of Test Inputs

| | Control | Treatment |
|---|---|---|
| Total users | 690 | 710 |
| Converted to paid | 22 | 50 |
| Conversion rate | 3.19% | 7.04% |

### Test Output

- **Z-statistic:** 3.264
- **One-tailed p-value:** 0.00055
- **Absolute lift:** +3.85 percentage points (Treatment − Control)
- **Relative lift:** +120.9%
- **95% confidence interval for the difference in conversion rate:**
  [1.56 pp, 6.15 pp] — entirely above zero

### Decision Rule

Reject H₀ if p < 0.05. Since p = 0.00055, which is far below 0.05, we **reject the
null hypothesis**.

### Business Interpretation

The new onboarding experience produced a statistically significant improvement in
paid conversion rate — more than double the Control group's rate, and this isn't
something that could plausibly be explained by random chance given how small the
p-value is. On the primary metric alone, this is a clear, strong result in favor
of the Treatment experience. However, per Task 8, this result should **not** be
read in isolation — guardrail metrics need to be checked before a launch decision
is finalized, since a conversion lift achieved by, say, pushing users through
onboarding too aggressively could come with hidden costs (see
`README.md` / `outputs/recommendation_memo.md` for the full guardrail discussion).
