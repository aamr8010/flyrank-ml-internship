# Content Opportunity Scoring for Human Review

## Abstract

This research analyzes which observable search and engagement signals can be used to prioritize content records for human review and optimization attention. Using a public-safe extract from the FlyRank ML Internship dataset, the analysis combines search visibility, clicks, traffic, and engagement measurements into a transparent opportunity score. The resulting workflow ranks content observations and assigns reason codes to support a practical review queue. The analysis shows that observable performance signals can be converted into a repeatable prioritization workflow without requiring client-identifying information or private search data. The results are intended as directional decision-support and do not establish causality or predict Google's ranking algorithm.

## 1. Introduction / Problem Statement

Content teams may have many content records to review but limited time to inspect every record equally.

This analysis addresses the decision of which content observations should receive human review first.

The goal is to create a transparent and repeatable prioritization workflow using observable search and engagement signals. The output is intended to reduce a large set of observations into a ranked review queue that can support editorial decision-making.

The analysis is decision-support rather than an automated publishing, ranking, or causal system.

## 2. Data

The analysis uses the FlyRank ML Internship dataset from the `fact_content_daily_performance` table.

The working extract contains 30,000 rows and 30 columns.

The observed reporting window is:

- Start: 2025-01-27
- End: 2025-02-22
- Unique clients: 3
- Unique content records: 5,673

The dataset contains observable search and engagement measurements including GSC impressions, GSC clicks, average position, GA4 sessions, engaged sessions, organic sessions, AI sessions, and related traffic signals.

Pseudonymous identifiers were used only for grouping and identifying records within the analysis. They were not used as predictive features.

No client names, domains, private queries, credentials, or other identifying information are exposed.

The available extract does not contain an explicit future-performance target, so no future outcome was invented.

## 3. Methodology

### Research Question

Which observable search and engagement signals can be used to prioritize content records for human review and optimization attention?

### Unit of Analysis

The primary unit of analysis is content observed on a reporting date.

### Engineered Features

The analysis derives several observable signals:

- Click-through rate (CTR)
- Engagement rate
- Organic traffic share
- AI traffic share
- Clicks per session

### Opportunity Score

A transparent weighted ranking score was created from percentile-ranked observed signals:

- 30% impressions
- 30% clicks
- 20% sessions
- 20% engagement rate

Higher scores indicate stronger observed signals and therefore higher priority for human review.

### Reason Codes

The ranking also provides reason codes based on observed signal thresholds, including:

- `high_visibility`
- `high_click_volume`
- `healthy_engagement`
- `strong_organic_share`
- `monitor`

### Target and Validation

There is no supervised future-outcome label in the available extract.

Therefore, this analysis does not claim supervised predictive performance, model accuracy, or a conventional model-versus-baseline comparison.

Potential leakage risks were considered by excluding pseudonymous identifiers from the scoring features and by avoiding future-outcome fields.

The resulting workflow should be interpreted as descriptive and directional decision-support.

## 4. Results

The analysis produced a ranked content-review queue from the available observations.

The highest-ranked records combine relatively strong observed visibility, click volume, traffic, and engagement signals.

The output includes a rank, opportunity score, reason code, observed performance signals, and a recommended action.

The top portion of the ranking is assigned to `human_review`, while lower-ranked observations remain in `monitor`.

Because the available extract does not contain an explicit future target or the Week-4 baseline output required for a valid same-split comparison, no supervised model-versus-baseline metric is reported.

The result should therefore be understood as a transparent ranking analysis rather than a predictive performance claim.

### Top 20 Opportunity Scores

![Top 20 Opportunity Scores](work/figures/top20_opportunity_scores.png)

The chart shows the opportunity scores of the highest-ranked content observations.

## 5. Limitations & Honest Framing

This analysis has several important limitations:

1. The extract does not contain an explicit future-performance target.
2. The opportunity score is a transparent ranking rule rather than a supervised prediction model.
3. The analysis measures observed signals and does not establish causality.
4. The reporting window is limited and does not capture long-term seasonality.
5. Pseudonymous identifiers cannot be interpreted as real URLs or client identities.
6. The output should be reviewed by a human before any content change.
7. The analysis should not be interpreted as predicting Google's ranking algorithm.
8. No automated content rewriting, deletion, merging, or publishing is recommended from this score alone.

The findings should therefore be described using terms such as observed, measured, directional, and decision-support.

## 6. Ranked Recommendations

### 1. Prioritize high-scoring records for human review

Start with records in the highest portion of the opportunity ranking because they combine stronger observed visibility, clicks, traffic, and engagement signals.

### 2. Review high-visibility records with weaker click capture

When impressions are relatively high but click volume is comparatively weaker, an editor can review titles, descriptions, search-intent alignment, and content positioning.

### 3. Protect strong-performing content

Records showing strong observed clicks and engagement should be reviewed carefully before making substantial changes.

### 4. Keep low-confidence records in monitoring

Records without strong supporting signals should remain in monitoring rather than receiving immediate content changes.

### Human Review Rule

The score should only prioritize review.

It should not automatically trigger rewriting, deletion, merging, publishing, or other irreversible content actions.

## 7. Reproducibility

The analysis is implemented in the capstone notebook:

`work/notebooks/capstone.ipynb`

The ranked action queue is available at:

`work/outputs/ranked_action_queue.csv`

The main visualization is available at:

`work/figures/top20_opportunity_scores.png`

The notebook contains the data validation, feature engineering, opportunity scoring, reason-code generation, ranked action queue, visualization, and artifact export steps.

## 8. Artifacts

The main generated artifacts are:

- Ranked action queue: `work/outputs/ranked_action_queue.csv`
- Opportunity-score visualization: `work/figures/top20_opportunity_scores.png`
- Capstone notebook: `work/notebooks/capstone.ipynb`

These artifacts are generated from the analysis workflow rather than manually edited.

## 9. Acknowledgments & Data Credit

Built on the FlyRank ML Internship dataset.

Data source: FlyRank.

The analysis uses the dataset provided for the FlyRank ML Internship and follows the public-safe framing requirements of the internship.
