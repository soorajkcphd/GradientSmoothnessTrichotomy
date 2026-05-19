# GradientSmoothnessTrichotomy

**Gradient Smoothness Trichotomy for Objectives Parameterized via Matrix Exponential Maps**

Experimental validation code for the paper:

> Sooraj K.C and Vivek Mishra
> "Gradient Smoothness Trichotomy for Objectives Parameterized via Matrix Exponential Maps"
> Submitted to *Journal of Applied Mathematics and Computing* (Springer), 2026

---

## What this code does

This script reproduces all tables, figures, and numerical claims in the paper.
It runs policy gradient experiments on SO(3) and SE(3) environments and
measures Fisher-metric alignment, convergence rates, computational efficiency,
empirical Lipschitz constants, and method comparisons.

The experiments correspond to paper sections and outputs as follows:

| Exp | What it tests                                    | Section     | Tables / Figures                    |
|-----|--------------------------------------------------|-------------|-------------------------------------|
| 1   | Fisher-metric alignment and isotropy tracking    | 7.1         | Table H1, Fig I1                    |
| 2   | Computational efficiency timing                  | 7.3         | Table 5 (timing)                    |
| 3   | Anisotropy ablation with controlled noise        | 7.1         | Fig H1 left                         |
| 4   | Sample efficiency: Lie policy vs ambient         | 7.1         | Fig H1 right                        |
| 5   | Convergence rate (deterministic)                 | 7.2         | Fig 1 left                          |
| 5b  | Alignment vs Fisher isotropy deviation           | 7.1         | (supporting data)                   |
| 6   | Scalability as K increases                       | 7.3         | Table 6 (scalability)               |
| 7   | SE(3) non-compact algebra comparison             | 7.4         | Table 4, Fig 2 left                 |
| 8   | Symmetry-violation robustness                    | Appendix J  | Table J6                            |
| 9   | Method comparison: LPG vs ambient PG vs NatGrad  | Appendix I  | Table I5, Fig I3                    |
| 10  | Convergence rate (stochastic)                    | 7.2         | Fig 1 right                         |
| 11  | Empirical Lipschitz constant L-hat(R) vs radius  | 7.5         | Fig 2 right                         |

Experiment 11 is the direct empirical validation of the trichotomy: it measures
the empirical Lipschitz constant L-hat(R) for so(3), gl(3), and se(3) on
semi-log axes and confirms the three predicted scaling regimes (O(1), O(e^{2R}),
and O(R^2) respectively). The key result: gl(3) shows factor ~56,700 growth
(exponential), while so(3) and se(3) remain bounded (factor <= 2).

---

## Setup

Python 3.8 or later is recommended.

Install dependencies:

    pip install -r requirements.txt

---

## Running experiments

Run all experiments:

    python experiments_unified.py

Run selected experiments (for example, 1, 7, and 11 only):

    python experiments_unified.py 1 7 11

Output figures are saved to the `figs/` folder. Numerical results are
printed to stdout.

---

## Output files

All figures are saved as PNG files at 300 DPI in `figs/`:

    exp1_fisher_alignment_hist.png          alignment distribution histogram
    isotropy_tracking.png                   eps_F, kappa, and alignment over training
    exp1_alignment_vs_eps.png               alignment vs Fisher isotropy deviation
    exp5_eps_vs_alignment.png               isotropy deviation vs alignment sweep
    controlled_anisotropy.png               anisotropy ablation results
    exp3_learning_curves.png                Lie vs ambient learning curves
    exp3_bar_stats.png                      bar chart of AUC statistics
    exp4_convergence_loglog.png             deterministic convergence log-log plot
    exp4_convergence_loglog_stochastic.png  stochastic convergence log-log plot
    exp2_timing.png                         Fisher inversion vs projection timing
    exp2_speedup.png                        speedup ratio across joint counts
    exp_se3_divergence.png                  SE(3) divergence with/without radius projection
    exp9_method_comparison.png              LPG vs ambient PG vs natural gradient
    exp9_wallclock_comparison.png           wall-clock time comparison
    exp11_empirical_lipschitz.png           empirical L-hat(R) vs R for so(3), gl(3), se(3)

---

## Code structure

Everything is in a single file (experiments_unified.py). The main components are:

    SO3, SE3                                Lie group utilities (hat, vee, exp, log, project)
    SO3JEnv, SE3Env                         RL environments for rotation and pose tracking
    LieLinearPolicy, ControlledNoisePolicy,
    DiagFeaturePolicy, AmbientLinearPolicy  policy classes
    train_reinforce, estimate_fisher,
    compute_fisher_metrics                  training and Fisher estimation utilities
    exp1 through exp10, exp5b,
    exp11_empirical_lipschitz               individual experiment functions
    run_all                                 entry point that runs selected or all experiments

---

## Reproducibility

All experiments use fixed random seeds. The global seed is set to 42 at
startup, and each experiment additionally seeds NumPy per trial. Results may
vary slightly across hardware due to floating-point differences in LAPACK
routines.

Expected runtime on a standard laptop CPU:

    Experiments 1, 2, 3, 6, 11:           a few minutes each
    Experiments 4, 5, 7, 8, 9, 10:        10 to 40 minutes each depending on hardware
    Full run (all experiments):            approximately 2 to 4 hours

All experiments were originally run on a single NVIDIA RTX 3090 GPU.
CPU runtimes will be longer.

---

## Citation

If you use this code, please cite:

    Sooraj K.C and Vivek Mishra.
    "Gradient Smoothness Trichotomy for Objectives Parameterized via
    Matrix Exponential Maps."
    Journal of Applied Mathematics and Computing, Springer (under review), 2026.

---

## Contact

Sooraj K.C
Department of Pure and Applied Mathematics, Alliance University, Bengaluru, India
ksoorajPHD23@sam.alliance.edu.in
