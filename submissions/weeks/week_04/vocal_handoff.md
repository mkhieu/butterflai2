# Project Vocal Handoff: Solar Emergence Modeling

## Executive Summary
We have successfully finalized the solar emergence modeling project, completing Tasks 37 through 43. The joint candidate model (`MY_MODEL`) has passed all anti-gaming fingerprinting protocols, physical plausibility diagnostics, and cross-validation benchmarks.

---

## Model Performance & Highlights

* **Candidate Model**: `emission-direct__profile-asymmetric__width-amplitude`
* **Student/Author**: Myra Khieu
* **Out-of-Sample Performance**:
  * **Paired Likelihood Gain**: **$+0.9213$ nats/hemicycle-year** over baseline.
  * **Wing Superiority**: Outperformed baseline on **$17$ out of $18$** held-out hemicycles.
  * **Goodness-of-Fit**: Median Kolmogorov-Smirnov ($KS$) statistic of **$0.0460$** ($8$ of $18$ wings unrejected at $\alpha = 0.05$).

---

## Key Achievements & Bug Resolutions

1. **Schema & Compatibility Fixes**:
   * Resolved `KeyError: 't0_map'` across cross-validation fold structures by implementing robust fallback checks (`fold_info.get("t0")` $\rightarrow$ `fold_info.get("t0_map")` $\rightarrow$ `T0[label]`).
   * Standardized plausible metric DataFrames to align with plotting schema expectations (`centroid_model` and `spread_model`).

2. **Cycle 19 Diagnostic Analysis (Task 42)**:
   * **Spatial/Temporal Residuals**: Identified slight under-predictions during the steep early rise phase ($s \in [-4, -2]\text{ yr}$) at mid-latitudes ($10^\circ\text{--}30^\circ$) and over-predictions in the low-latitude post-peak decay tail.
   * **Butterfly Diagrams**: Synthetic emergence events generated via 2D thinning accurately reproduced observed latitudinal drift patterns.

3. **Leaderboard & Report Infrastructure (Task 43)**:
   * **Fingerprint Anti-Gaming**: Integrated `protocol_fingerprint()` validation into `write_report()`.
   * **Slugified Identity Resolution**: Configured dynamic identity detection (`BUTTERFLAI_STUDENT` / git config) resulting in the clean export slug `scoreboard1_report_myra-khieu.json`.
   * **Leaderboard & Refusal Logic**: Built `class_leaderboard()` and `draw_class_board()`, correctly rejecting bound models and mismatched fingerprints.
   * **Cleanup**: Deleted stale placeholder reports (`scoreboard1_report_unset.json`).

---

## Active Workspace Deliverables

* `submissions/weeks/week_04/scoreboard1_report_myra-khieu.json` (Validated & Fingerprinted)
* Figure 10: Cycle 19 2D Residual Maps & 1D Marginal Projections
* Figure 11: Class Leaderboard with Refusal Panel

---

## Next Steps

* **Git Commit & Push**: Ensure your report file is pushed to your remote repository:
  ```bash
  git add submissions/weeks/week_04/scoreboard1_report_myra-khieu.json
  git commit -m "Submit final fingerprinted Task 43 scorecard for Myra Khieu"
  git push origin main
  ```