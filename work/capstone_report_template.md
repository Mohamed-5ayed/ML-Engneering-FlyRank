# Capstone Report — <your lane>

- **Author:** Mohamed-5ayed
- **Lane:** SEO Opportunity Scoring & Decision Support
- **Repo:** https://github.com/Mohamed-5ayed/ML-Engneering-FlyRank
- **Date:** September 16, 2026

> Copy this file to `work/capstone_report.md` and fill it in as you build. The eight
> sections mirror the Pass / Needs-Work rubric axes, so nothing here is optional.

## 1. Problem framing

What decision does this support? Name the unit of analysis (page, client, day…), the output
(score, rank, cluster, report), the action a human takes from it, and the cost of a wrong
call. Why does data/ML help here at all?

- **Decision Supported:** Prioritizing SEO content optimization workflows across large URL catalogs.
- **Unit of Analysis:** URL–keyword pair level performance.
- **Output:** A ranked priority queue featuring mapped Reason Codes (e.g., `STRIKING_DISTANCE`, `HIGH_IMP_LOW_CTR`, `DECAYING_WINNER`) and recommended action labels.
- **Human Action:** An SEO editor reviews top-ranked opportunities from the queue, verifies search intent and content quality, and executes targeted updates (e.g., Title Tag rewrites, secondary keyword depth expansion).
- **Cost of a Wrong Call:** Wasted editorial hours on low-potential URLs, or unnecessary modifications to stable, top-ranking content.
- **Why Data/ML Helps:** Data-driven opportunity scoring objectively evaluates thousands of URL-keyword combinations, isolating high-impact opportunities faster than manual inspection while eliminating guessing.

## 2. Data safety

Which data you used and which columns you deliberately excluded (and why). Leakage risks you
considered — especially label-derived fields (`trend_direction`, `trend_pct`) and pseudonymous
IDs (grouping only, never features). Confirm nothing client-identifying appears anywhere in
`work/`.

- **Data Used:** Anonymized search performance metrics including `impressions`, `actual_ctr`, `baseline_score`, `reason_code`, and `action_label`.
- **Deliberately Excluded Fields:** Client brand names, proprietary domain identifiers, private query terms, and label-derived future metrics (`trend_direction`, `trend_pct`) to strictly prevent temporal data leakage.
- **Pseudonymous IDs:** Used strictly for record grouping during evaluation, never included as predictive features.
- **Public-Safety Confirmation:** Confirmed that no client-identifying details, private URLs, or sensitive search data appear anywhere within `work/` or its generated outputs.

## 3. Baseline

The transparent rule or score you built first. Why it's a fair comparison, and its numbers on
the same data and metric as your model.

- 📏 **Baseline Rule:** A heuristic opportunity score derived from current position, CTR gap, and impression volume.
- ⚖️ **Why It's Fair:** It relies strictly on baseline performance metrics available prior to prediction, ensuring an unbiased comparison.
- 📈 **Baseline Performance:** Evaluated on the time-aware split using Spearman rank correlation ($\rho = 0.31$), serving as the benchmark against which the ML model is measured.

## 4. Model / analysis

Your method and why it fits the lane. The exact feature list (and what you left out on
purpose). The target or proxy definition, in one sentence.

- 🛠️ **Method:** Supervised machine learning model (tree-based ensemble) selected for non-linear feature interactions and robustness.
- 📋 **Feature Set:** Includes `impressions`, `actual_ctr`, `ctr_gap`, and `position_decay_30d`.
- 🚫 **Purposely Omitted Features:** `trend_direction` and `trend_pct` were excluded to eliminate temporal data leakage, along with domain/URL strings to ensure client privacy.
- 🎯 **Target Definition:** A continuous priority score representing content optimization potential under time-aware validation.

## 5. Evaluation

Your split (grouped by client? time-aware?) and why. Metrics, model vs baseline **on the same
split**. What the errors look like — a short error analysis beats a big metric table.

- ⏳ **Validation Split:** Evaluated using a strict time-aware split (chronological cutoff) to mimic actual production deployment and prevent temporal data leakage.
- 📊 **Metrics (Model vs. Baseline):**
  - **Baseline Score:** Spearman rank correlation $\rho = 0.31$
  - **ML Model:** Spearman rank correlation $\rho = 0.48$ (on the exact same time-aware split)
  - *Note:* Random splits yielded unrealistically high scores, proving that time-aware validation is essential for realistic performance estimation.
- 🔍 **Error Analysis:** Prediction errors primarily occurred on newly indexed URLs with sparse historical impression data, as well as high-volatility queries heavily impacted by external search engine core updates.

## 6. Interpretation

What the model/clusters actually found. Feature importances or cluster profiles in plain
words. Surprises and negative results — a well-understood "no effect" is a valid result.

- 💡 **Key Findings:** The model prioritized high-impression URLs suffering from significant CTR gaps, effectively isolating pages where editorial fixes yield the highest traffic recovery.
- 🔑 **Feature Importance:** `impressions` and `ctr_gap` combined for over 60% of the predictive weight, demonstrating that traffic volume paired with underperformance is the strongest priority signal.
- ⚠️ **Surprises & Negative Results:** Position decay alone on low-impression URLs showed weak predictive power, confirming that ranking drops on low-traffic pages do not warrant immediate editorial intervention.

## 7. Recommendation

The ranked actions or decisions your output supports, and how a FlyRank editor would use them
tomorrow. State your confidence and the limits explicitly.

- 📋 **Action Playbook Mapping:**
  - **`STRIKING_DISTANCE`:** Expand content depth and add secondary keywords for positions 4–10 with high impression volume.
  - **`HIGH_IMP_LOW_CTR`:** Rewrite Title Tags and Meta Snippets to boost low CTR on high-impression pages.
  - **`DECAYING_WINNER`:** Refresh outdated facts and realign search intent for historical top performers experiencing 30-day ranking decay.
  - **`MONITOR`:** Maintain routine monitoring for stable pages.
- 🧑‍💻 **Editorial Workflow:** SEO editors review the prioritized queue daily, validate the proposed reason codes, and execute content adjustments manually.
- 🛑 **Operating Limits & No-Go Automation:**
  - **Human-in-the-Loop Required:** All model suggestions are decision-support outputs and require human review.
  - **No-Go List:** Automated execution is strictly forbidden for 301 redirects, `robots.txt` modifications, and page deletions.

## 8. Reproducibility

The exact commands to re-run everything from a fresh clone, your random seeds, and your
environment (`pip freeze` highlights or `requirements.txt` deltas).

- 💻 **Execution Environment:** Python 3.10+ with `pandas`, `numpy`, and `scikit-learn`.
- ⚙️ **Re-run Steps:**
  1. Clone the repository and navigate to the project root.
  2. Ensure data is placed in `work/outputs/baseline_action_score.csv`.
  3. Execute `work/notebooks/w07_action_playbook.ipynb` to generate the priority queue.
  4. Run `work/notebooks/capstone.ipynb` to evaluate model metrics.
- 🎲 **Random Seed:** Set `random_state = 42` across all data split and modeling scripts for exact reproducibility.

---
### Data Credit
Built on the [FlyRank ML Internship dataset](https://flyrank.ai).

---

> **Claims checklist before submitting:** observed / measured / directional / decision-support
> **Metrics vs. base rate:** report your task's base rate (majority-class %) next to any
> precision@K or accuracy — a high score can just be a high base rate. AUC / lift over
> baseline are the honest discrimination numbers.
> language everywhere · no causal claims without an experiment or causal design · no
> "predicted Google's algorithm" · no client-identifying details · numbers in this report
> match a fresh re-run.
