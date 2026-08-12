# ML-08 — Capstone Report

- **Author:** Ahmed Amr
- **Lane:** Search Intelligence / Content Action Prioritization
- **Repo:** https://github.com/aamr8010/flyrank-ml-internship.git
- **Date:** 2026-08-12

## 0. Abstract

This project asks whether observed search and engagement signals can support a practical prioritization of content pages for human review. The analysis uses the FlyRank ML Internship dataset and focuses on anonymized content-level and daily search-performance observations, using a 30,000-row working extract for the modeling workflow. A transparent rule-based baseline and interpretable machine-learning analysis were used to study which observed signals can provide useful directional decision support without making causal claims. The results are interpreted as measured and observed signals rather than proof that refreshing or changing a page will improve its future performance. The final output is a ranked content-action queue intended to help an editor decide which pages deserve human review first.

## 1. Problem framing

The decision supported by this work is **which content pages should receive human review first**.

The unit of analysis is an observed content-level record containing search, engagement, and traffic signals over the available reporting window.

The output is a ranked decision-support queue containing a score, reason code, and suggested action.

A human editor can use the ranking to prioritize review of pages showing combinations of signals such as high search visibility with relatively weak click or engagement signals.

The cost of a wrong call is mainly wasted editorial effort or an inappropriate content change. Therefore, the output should be treated as a prioritization aid rather than an automated decision.

ML and structured scoring are useful because the dataset contains multiple signals that can be considered together. The goal is to make those signals easier to compare and review consistently.

## 2. Data safety

The analysis uses the FlyRank ML Internship dataset and a 30,000-row working extract loaded from the authenticated dataset release.

The available modeling data included fields such as:

- `report_date`
- `client_hash_id`
- `content_hash_id`
- `gsc_impressions`
- `gsc_clicks`
- `gsc_avg_position`
- `ga4_pageviews`
- `ga4_sessions`
- `ga4_users`
- `ga4_engaged_sessions`
- `sessions_organic`
- `sessions_ai`
- AI-source traffic fields
- `scroll_events`

The pseudonymous identifiers `client_hash_id` and `content_hash_id` were used only to distinguish or group observations where required. They were not used as predictive features.

Fields that could encode information derived from later observations, such as `trend_direction` and `trend_pct` when present in derived releases, were deliberately excluded from predictive features because they can create leakage when the prediction or decision is intended to happen before that information is known.

Client-identifying information, private queries, domains, and URLs were not included in the public-facing analysis.

The analysis does not attempt to identify clients, domains, search queries, or individuals.

## 3. Baseline

The first decision-support baseline was a transparent rule-based score.

The baseline combines observable signals including search visibility, click-through behavior, engagement, and AI-traffic signals.

The ranking gives greater priority to pages with high visibility combined with relatively weak CTR or engagement signals, while also recognizing pages with a strong observed AI-traffic signal.

The baseline is useful because its logic is explicit and easy for a human reviewer to inspect.

The baseline should be treated as a directional prioritization rule rather than a prediction of a confirmed business outcome.

Where a direct model-versus-baseline metric could not be supported by the available aligned target, I did not invent a metric or target.

## 4. Model / analysis

The modeling work explored an interpretable Decision Tree approach because the project requires a method that can be inspected and compared with a transparent baseline rather than maximizing complexity.

The available signals considered for analysis included:

- `gsc_impressions`
- `gsc_clicks`
- `gsc_avg_position`
- `ga4_sessions`
- `ga4_engaged_sessions`
- `sessions_organic`
- `sessions_ai`
- engagement-related measures
- other observed traffic and availability signals where appropriate

Pseudonymous IDs were excluded from predictive features.

Future-derived fields were excluded when they could expose information unavailable at the intended decision moment.

The target limitation is important: the 30,000-row daily extract did not contain a direct confirmed business-outcome label corresponding to "successful refresh." Therefore, I do not claim that the model predicts whether refreshing a page will improve performance.

The modeling output is therefore framed as **decision support based on observed signals**, not as a causal or business-outcome prediction.

## 5. Evaluation

The evaluation was designed to avoid treating the same observations used to construct a decision rule as independent evidence of future improvement.

The available working extract covered:

- **Rows:** 30,000
- **Date range:** 2025-01-27 to 2025-02-22
- **Unique client and content identifiers:** anonymized

Where a valid supervised target and aligned baseline were unavailable, I did not fabricate accuracy, AUC, precision, recall, or lift numbers.

This is an important limitation of the current release: the available extract supports signal analysis and decision-support ranking, but it does not by itself provide a confirmed future outcome for evaluating whether a recommended refresh actually worked.

The error analysis therefore focuses on weak or potentially misleading ranking cases rather than pretending that an unavailable outcome label exists.

Typical failure cases include:

1. A page may have high impressions and low CTR for a legitimate reason, such as search intent or SERP competition, rather than because its content needs refreshing.
2. A page may have low engagement because the user successfully found the required information quickly.
3. AI traffic may be high without implying that the page requires a content change.
4. A ranking can prioritize a page because of a statistical signal that a human editor would reasonably reject after reviewing the actual content.

These cases reinforce the requirement for human review.

## 6. Interpretation

The analysis indicates that search visibility, CTR, engagement, and AI traffic can provide useful directional signals for prioritization.

High visibility combined with relatively weak CTR is a reasonable reason to inspect search-result presentation and intent alignment.

High visibility combined with relatively weak engagement can justify checking whether the content satisfies the visitor's needs.

Observed AI traffic can be useful as an additional context signal, but it should not independently trigger an automated content change.

An important negative result is that the available extract does not support a causal statement that any of these signals proves a refresh will improve performance.

This is a useful result because it prevents the system from turning correlation or observed behavior into an unsupported business claim.

The main interpretation is therefore:

> The signals can help prioritize human investigation, but they do not prove which editorial intervention will produce a future performance improvement.

## 7. Recommendation

The recommended workflow is:

### 1. Review high-visibility / low-CTR pages

**Reason:** strong observed search visibility combined with relatively weak click-through behavior.

**Human action:** inspect title, snippet, search intent, and SERP context.

**Confidence:** directional.

**Limit:** low CTR does not prove that the page needs a content change.

### 2. Review high-visibility / low-engagement pages

**Reason:** strong observed visibility combined with relatively weak engagement.

**Human action:** inspect content usefulness, page experience, and intent alignment.

**Confidence:** directional.

**Limit:** low engagement can have legitimate explanations.

### 3. Review pages with notable AI traffic

**Reason:** measurable observed traffic from AI-related sources.

**Human action:** inspect whether the content is clear, useful, current, and appropriate for users arriving through these channels.

**Confidence:** exploratory/directional.

**Limit:** AI traffic alone does not establish that a page should be changed.

### 4. Monitor weak-signal pages

**Reason:** insufficient evidence for a stronger action.

**Human action:** monitor rather than immediately spend editorial effort.

**Confidence:** low.

**Limit:** a low score does not mean the page has no business value.

### Human-review principle

The ranked output should be treated as a queue for editors, not an automated publishing system.

The editor should review the actual page and context before taking action.

The system should **not** automatically:

- rewrite content;
- publish content;
- delete pages;
- change canonical URLs;
- change metadata without review;
- make client-facing decisions;
- claim an improvement will occur;
- infer Google's ranking algorithm.

## 8. Reproducibility

The project is organized as a sequence of notebooks under `work/notebooks/`.

The main workflow includes:

- `w03_data_contract.ipynb`
- `w04_baseline_score.ipynb`
- `w05_model.ipynb`
- `w06_validation_audit.ipynb`
- `w07_action_playbook.ipynb`
- `capstone.ipynb`

The dataset is loaded through the authenticated Hugging Face dataset release using a Colab secret named `HF_TOKEN`.

The working modeling extract contains 30,000 rows.

The analysis uses Python with pandas, NumPy, and scikit-learn where required.

The notebooks should be executed from top to bottom so that derived outputs and metrics are regenerated from the source data rather than manually entered.

The random state used in any randomized modeling split should remain fixed in the corresponding notebook so that the result is reproducible.

Generated data files such as ranked queues remain outside git where required by the project leak-guard. Metrics JSON files and reusable figures should be committed as project receipts.

## 9. Acknowledgments & data credit

Built on the FlyRank ML Internship dataset.

Data source: [FlyRank](https://flyrank.ai)

This project uses the dataset for educational research and decision-support analysis. The analysis does not attempt to identify individual clients, domains, private queries, or other private information.
