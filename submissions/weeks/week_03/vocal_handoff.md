# ButterflAI 2.0 — Week 3: Latent Magnetic Belt

## Executive Summary
Week 3 transitions the ButterflAI framework from empirical 1D event-rate curves to a 2D spatial-temporal point-process model. In this framework, an unobserved latent magnetic belt $B(|\lambda|, s)$ triggers sunspot emergences via a buoyancy threshold $B_{\text{crit}}$.

All major implementation tasks (Tasks 20–31) in `03_latent_belt.ipynb` have been implemented, executed, and validated.

---

## Key Achievements & Technical Milestones

### 1. Clock Standardization & Leakage Prevention (Tasks 20–21)
* **`t0_crossing`**: Estimates initial $15^\circ$ downward latitude crossing times $t_{0,\text{start}}$ using only each hemicycle's internal event catalog.
* **`fit_alignment`**: Fits a shared exponential drift timescale ($\mu_b \approx 7.2\text{–}7.3\text{ years}$) and per-hemicycle time alignment shifts $\delta_{\text{hc}}$ across all 16,611 development events simultaneously without data leakage.

### 2. 2D Surface Mechanics & Frozen Integration Mesh (Tasks 22–24)
* **Latent Belt Surface $B(|\lambda|, s)$**: Modeled as a Gaussian band in latitude whose peak moves along the central drift track $\mu(s) = 15^\circ \exp(-s / \mu_b)$.
* **Softplus Emission $\Lambda(|\lambda|, s)$**: Maps latent field strength to emergence intensity using fixed gauge choices ($B_{\text{crit}} = 1.0, \alpha = 1.0$).
* **Frozen Quadrature Mesh (`MESH_2D`)**: Fixed at $201 \times 241$ nodes ($s \in [-6, +11]\text{ yrs}$, $|\lambda| \in [0^\circ, 60^\circ]$) to eliminate systematic coarse-mesh flattery bias across model variations.

### 3. Pooled Fit Engine (Tasks 25–26)
* **`fit_pooled`**: Alternating optimizer using `L-BFGS-B` for 6 shared shape parameters and bounded 1D scalar optimization for individual hemicycle belt amplitudes $a_{\text{hc}}$.
* **Closed-Form Scale $c$**: Re-solves the global emission scale parameter $c$ in closed form during each optimization sweep.

### 4. Cross-Validation & Metric Robustness (Tasks 27–29)
* **2D Thinning Simulator (`simulate_wing`)**: Implemented Lewis–Shedler rejection sampling to synthesize butterfly emergence patterns from fitted surfaces.
* **Leave-One-Cycle-Out (LOCO)**: Evaluated 9 folds where both Northern and Southern hemicycles of a cycle are held out simultaneously, with clock parameters refitted purely inside each fold.
* **Anti-Gaming Sensitivity**: Demonstrated that altering mesh boundaries or node density shifts raw log-likelihood scores significantly, reinforcing the requirement for a strictly frozen ruler.

### 5. Belt Width Hypothesis Testing & Results (Tasks 30–31)
* **Sample-Size Artifact**: Observed latitudinal spread scales positively with event count $N$ (slopes $+0.34$ to $+0.39$), proving that apparent belt widening in large cycles is an artifact of Poisson sampling rather than physical broadening.
* **Held-Out Model Selection**: Compared constant width ($w \approx 6.5^\circ$), amplitude-dependent width $w(a_{\text{hc}})$, and latitude-dependent width $w(\mu)$.
* **In-Sample vs. Held-Out Ranking Reversal**: Complex amplitude-dependent rules suffered severe out-of-sample overfitting (collapsing on Cycle 19N). Paired held-out likelihood comparisons establish the parsimonious **constant-width belt ($w \approx 6.5^\circ$)** as the superior physical model.

---

## Reference Parameters & Fixed Domain

| Parameter / Domain | Value / Setting |
| :--- | :--- |
| **Time Domain ($s$)** | $s \in [-6.0, +11.0]\text{ years}$ ($N_s = 201$) |
| **Latitude Domain ($|\lambda|$)** | $|\lambda| \in [0^\circ, 60^\circ]$ ($N_{\lambda} = 241$) |
| **Gauge Conventions** | $B_{\text{crit}} = 1.0$, $\alpha = 1.0$, $\mu_{\text{ref}} = 15.0^\circ$ |
| **Drift Timescale ($\mu_b$)** | $\approx 7.2\text{--}7.3\text{ years}$ |
| **Baseline Belt Width ($w$)** | $\approx 6.5^\circ$ |