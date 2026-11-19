# Ultimate Technologies | Demand, toll reimbursement and rider retention

## 1. When do riders log in?

The file contains 93,142 logins between January 1 and April 13, 1970. Across complete dates, weekday demand peaks at 11:30 a.m. (20.1 logins per 15 minutes), while the weekend peak moves to 4:45 a.m. (35.6). Saturday averages 13.5 logins per interval across the entire day, more than double Monday’s 6.3. A staffing plan based on a single daily average would miss the early weekend surge. Logins measure app activity; completed trips would be needed to estimate ride demand directly.

![15-minute login counts across the observation window, with separate weekday and weekend profiles underneath](figures/demand.png)

Each timestamp counts as one login. The analysis sorts the events, fills 407 empty intervals with zero observed logins and retains 877 additional events sharing an exact timestamp. Without user IDs, simultaneous logins cannot be distinguished from duplicate records. Partial first and last dates appear in the time series and are excluded from the daily profiles. The source gives no timezone or outage log.

## 2. Would reimbursing bridge tolls change driver behavior?

**Primary outcome.** For each eligible driver, calculate the fraction of pilot weeks in which they completed at least one pickup in each city. Average that fraction over all assigned drivers, including drivers with no pickups. A crossing with no passenger service earns no credit. Using a fixed denominator also captures any effect on whether drivers work.

**Pilot.** Enroll drivers who completed a ride in the four weeks before launch. Within home city and baseline cross-city activity strata, randomly assign drivers equally to automatic two-way toll reimbursement or existing terms. Run both arms on the same dates for at least four full weeks, link toll records to active driver accounts and record failed reimbursements. Before launch, use historical driver-week data to estimate the variance of the per-driver outcome and size the pilot for an agreed minimum worthwhile lift at 80% power and a two-sided 5% significance level.

**Analysis and decision.** Compare assignment groups by intention to treat. Estimate the within-stratum difference in mean driver outcomes, weighted by enrolled stratum size. Permute driver assignments within strata for a two-sided randomization test. Bootstrap drivers within strata and arms for a 95% interval. This preserves the dependence between weeks for the same driver. Track pickup wait times in each city, unserved requests, and reimbursement cost per additional driver-week of dual-city service. Expand if the estimated gain is economically worthwhile, its interval supports a positive effect, and both cities meet service and cost limits agreed before launch. An interval spanning negligible and worthwhile effects calls for more evidence. Drivers compete for the same rides, so check a phased market rollout for displacement before forecasting the full-scale gain.

## 3. Can first-month information predict June activity?

**Outcome.** January signups form a cohort of 50,000 riders. Using July 1 (the latest recorded trip) as the final observed date, 18,310 riders, or **36.62%**, took a trip during June 2–July 1 inclusive. Counting June 1 as well gives 37.61%. This snapshot approximates month-six activity. Exact rider-relative sixth-month labels require dated trip histories.

**Predictors.** The model uses signup city, first-30-day trips, first-30-day average distance and first-30-day Ultimate Black use. All four are complete. Lifetime ratings, weekday mix and surge measures may include future trips. Phone is recorded as the primary device without a dated snapshot. We excluded these variables. Retention ranged from 24.3% in Astapor to 62.3% in King’s Landing. Among riders with one first-month trip, 21.6% were retained, compared with 63.6% for five or more. The zero-trip group retained at 34.7%, so risk does not change linearly with trip count.

| Method | ROC AUC | Average precision |
|---|---:|---:|
| Constant prevalence score | 0.500 | 0.368 |
| Logistic regression | 0.726 | 0.615 |
| Histogram gradient boosting | 0.761 | 0.647 |

The first 24 signup dates trained the models; the final seven dates supplied 11,881 held-out riders. Boosting can learn the non-monotonic trip relationship. At a 0.50 retention threshold it identified 2,102 of 4,368 retained riders (48.1% recall, 66.1% precision). Shuffling city reduced held-out AUC by 0.099, followed by first-month trip count (0.063), distance (0.044) and Black use (0.041). These estimates reflect one cohort and a retrospective split. A later cohort with completed follow-up is needed to test prospective validity.

**Action.** At day 30, pilot a second-ride offer among one-trip riders. Randomize the offer within city and compare incremental retained riders and contribution margin after incentive costs. City differences can guide investigation into pickup reliability and supply. The 49.6% vs 28.7% retention gap by Black use alone cannot establish that upgrading a rider would improve retention.

## Reproduce

Use Python 3.11 or later. Put the supplied logins.json and ultimate_data_challenge.json in a local data/ folder, install requirements and run from this directory:

    python -m pip install -r requirements.txt
    python -m jupyter lab Ultimate_Rider_Retention.ipynb

The notebook displays the tables and regenerates the figure. Remove the rider dataset after running, as requested in the challenge brief.

Source: Springboard take-home challenge. Analysis used the [public copy of the supplied inputs](https://github.com/pjandir/Springboard-DSTrack/tree/32c476cef31c84840a770e4396e2b76acfb9a71d/Exercises/take_home_challenges_20.1/ultimate_challenge).
