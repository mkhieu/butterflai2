# Session Handoff Summary: Solar Active Region Emergence Analysis

## Overview
This session focused on implementing and debugging the **Inhomogeneous Rate-Matched Null Model** for analyzing overdispersion ($F(\Delta) > 1$) in solar active region emergence times. The analysis progresses from testing against a constant-rate homogeneous Poisson baseline to a rate-matched null model that preserves the 11-year solar cycle envelope.

---

## Key Methodology & Workflows

### 1. Homogeneous vs. Inhomogeneous Poisson Nulls
* **Homogeneous Null (`simulate_homogeneous_poisson`)**:
  * Assumes a constant emergence rate $\lambda = N / T$.
  * Serves as the standard benchmark ($F(\Delta) \approx 1.0$).
* **Inhomogeneous Rate-Matched Null (`simulate_rate_matched_null`)**:
  * Preserves broad time-varying activity (solar cycle) while destroying short-term spatial/temporal clustering or memory.

### 2. Implementation Steps for `simulate_rate_matched_null`
1. **Step 1 (Daily Binning)**: Histogram continuous event times $s$ into 1-day bins spanning $[s_{\text{lo}}, s_{\text{hi}}]$:
   $$\text{num\_days} = \lfloor s_{\text{hi}} - s_{\text{lo}} \rceil$$
2. **Step 2 (Boxcar Smoothing with Edge Correction)**:
   * Smooth daily counts using a boxcar kernel of width `smooth_days` (e.g., 91 days or 365 days).
   * Divide by a boxcar-smoothed array of ones (`edge_weights`) so days near the start and end of the dataset are not artificially pulled down toward zero due to truncated window coverage.
3. **Step 3 (Poisson Resampling)**:
   * Draw a new count $k_t \sim \text{Poisson}(\mu_t)$ for each day $t$, where $\mu_t$ is the smoothed daily rate.
4. **Step 4 (Uniform Scattering & Output)**:
   * For days with $k_t > 0$, scatter event times uniformly within $[t, t+1]$.
   * Concatenate, sort chronologically, and return a 1D NumPy array.

---

## Critical Debugging & Fixes

* **TypeError (`'<' not supported between instances of 'NoneType' and 'float'`)**:
  * *Root Cause*: `simulate_rate_matched_null` returned `None` instead of an array, which caused `searchsorted`/`histogram` downstream in `fano_curve` to crash when attempting to operate on `None`.
  * *Fix*: Ensured proper indentation of the return block so that a valid 1D NumPy array (or `np.array([])` for empty fallback) is returned across all execution paths.

---

## Task 9 Visualization & Analysis Context

* **Observed Baseline (`fano_obs`)**:
  * Derived from `fano_data['north']` / `fano_data['south']` computed via `fano_curve`.
* **Null Comparison**:
  * Comparing $F_{\text{obs}}(\Delta)$ against synthetic realizations averaged across `N_REALIZATIONS`.
  * Plotting Fano curves ($F(\Delta)$ vs. $\Delta$) on log-log scales highlights the onset of overdispersion across varying temporal scales $\Delta$.

---

## Next Steps
* Proceed with running Task 9 execution cells using `null_curves` and plotting the ratio or comparison curves of observed Fano factors versus 3-month and 1-year null models.