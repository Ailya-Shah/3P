---
layout: post
title: "Basel-Lite: Building a Credit-Risk Engine — Then Validating My Own Model and Provisioning It Under IFRS 9"
date: 2026-07-08
tags: [data-science, credit-risk, machine-learning, ifrs9, model-validation, python]
excerpt: "I built a credit model, then did the two things most portfolio projects skip: I validated it as if I were the independent reviewer, and I turned it into a forward-looking IFRS 9 loss allowance. Here's the reasoning, the honest findings, and the parts I chose not to fake."
---

<!--
  IMAGE PATHS: this post expects screenshots in /assets/basel-lite/
  If your Jekyll assets live elsewhere, change the prefix once and it propagates.
  Every image below uses:  {{ "/assets/basel-lite/NAME.png" | relative_url }}
-->

Most credit-risk portfolio projects stop at "I trained a classifier and it got 0.7 AUC." I wanted to build the thing a bank actually runs — not just the model, but the two layers that sit on top of it in a real risk function: an **independent validation** of the model, and a **forward-looking loss allowance** under IFRS 9. This is the story of Basel-Lite, including the parts that didn't flatter me, because those are the parts that taught me the most.

![Basel-Lite borrower scorer]({{ "/assets/basel-lite/hero-dashboard.png" | relative_url }})

## At a glance

| | |
|---|---|
| **Data** | LendingClub 2007–2018, 200K sample (≈119K completed loans) |
| **Model** | Calibrated LightGBM PD · WoE scorecard (300–850) · recovery-based LGD |
| **Discrimination** | AUC 0.713 · Gini 0.426 · KS 0.308 |
| **Calibration** | Expected calibration error 0.009 |
| **Validation** | 8-check independent pipeline, reproducible + tested |
| **IFRS 9 ECL** | 5.75% portfolio coverage, staged and scenario-weighted |
| **Stack** | MySQL · LightGBM · FastAPI · Streamlit · pytest |
| **Tests** | 16 passing across two packages |

Repo: [github.com/Ailya-Shah/basel_lite](https://github.com/Ailya-Shah/basel_lite) · Live dashboard: *(link)*

---

## The problem

Lending isn't the hard part. Pricing the risk *before* the outcome is known is. A bank needs three numbers for every loan — how likely is default (**PD**), how much is lost if it happens (**LGD**), and how much is exposed (**EAD**) — and it needs them to be *trustworthy enough to reserve real capital against*. Basel-Lite is my attempt to build that chain end to end on real data, and then to stress-test my own work the way a second line of defence would.

---

## Part 1 — The model

I trained a **calibrated LightGBM** to estimate PD, using **only features known at application time**. This mattered more than any hyperparameter: LendingClub's file is full of post-origination fields (`recoveries`, `total_pymnt`, last-pull FICO) that leak the outcome. Dropping them is the difference between an honest 0.71 AUC and a fantasy 0.95.

On top of the PD model I built a **Weight-of-Evidence scorecard** scaled to the familiar 300–850 range — the interpretable, points-based form regulated credit scores actually take — and measured **LGD empirically** from real recoveries on charged-off loans rather than assuming a number. Those combine into Expected Loss, `EL = PD × LGD × EAD`.

![ROC curve]({{ "/assets/basel-lite/roc-curve.png" | relative_url }})

The single most important chart isn't the ROC, though — it's the calibration curve.

![Calibration curve]({{ "/assets/basel-lite/calibration-curve.png" | relative_url }})

An **expected calibration error of 0.009** means my predicted PDs land within about one percentage point of the observed default rate across the whole range. That's what licenses feeding them straight into the loss math: a model can rank borrowers perfectly and still be uncalibrated, and an uncalibrated 5% that's really 12% quietly wrecks every downstream capital number. Discrimination gets the headlines; calibration is what makes the money math valid.

I also used **SHAP** to explain every prediction and **survival analysis** (Kaplan–Meier) to model *when* defaults happen, not just whether — a curve that turns out to matter a lot in Part 3.

![SHAP feature importance]({{ "/assets/basel-lite/shap-importance.png" | relative_url }})

The whole thing ships as a real stack: a MySQL data layer, a FastAPI scoring service, and a Streamlit dashboard where dragging a borrower's FICO recalculates their risk and expected loss live.

({{ "/assets/basel-lite/portfolio-risk.png" | relative_url }})

---

## Part 2 — Validating my own model

Here's where I did something most projects don't: I built an **independent validation layer** that treats my model as a suspect, not a trophy. It's a separate `validation/` package that re-scores a fixed-seed holdout the exact way the API does, runs eight acceptance checks with explicit thresholds, encodes each as a `pytest` test, and renders a PDF report. A *failing* check is a documented finding, not a crash.

![Validation report — summary]({{ "/assets/basel-lite/validation-report-cover.png" | relative_url }})

All eight pass on the LendingClub holdout: leakage, discrimination (Gini/KS), calibration, in-sample stability, **out-of-time stability** (PSI 0.008 comparing 2013–14 vs 2015–16 loan vintages — the "is my model still valid two years later" question), rank monotonicity, and a champion-vs-challenger benchmark.

![Validation report — evidence]({{ "/assets/basel-lite/validation-report-evidence.png" | relative_url }})

**The most honest result in the whole project lives here.** My LightGBM beats a plain WoE-logistic regression by only **0.007 Gini**. I could have buried that. Instead the report states it plainly: most of the signal is already captured linearly through the WoE transform, so the simpler, more interpretable logistic model would be a defensible production choice — the gradient boosting earns its keep only at the margin, and mostly for the explainability tooling around it. Learning to *report the unflattering delta* instead of hiding it is, I think, the actual skill a model-validation team hires for.

---

## Part 3 — Provisioning under IFRS 9

A single PD tells you whether a loan defaults. **IFRS 9** asks a harder question: how much loss should you reserve *today*, looking forward, over the right horizon? The `ecl/` engine answers it. For each loan and each macro scenario:

```
ECL = Σ over months [ marginal PD(t) × LGD × EAD(t) × discount(t) ]
```

then probability-weights the scenarios. It reuses everything upstream:

- **PD term structure** — I use my survival curve to spread each loan's lifetime PD across the months of its term, so I have a *marginal* PD per month that sums back to the total. My LightGBM gives the *level*; the survival curve gives the *timing*.
- **Amortization schedule** — EAD declines month by month as the loan is repaid, instead of the flat loan-amount shortcut.
- **Staging** — Stage 1 (12-month ECL) vs Stage 2/3 (lifetime), which decides the horizon.
- **Vasicek macro overlay** — shifts through-the-cycle PD to point-in-time under upside / baseline / adverse scenarios.

![IFRS 9 ECL report — summary]({{ "/assets/basel-lite/ecl-report-cover.png" | relative_url }})

On the full completed book (119,060 loans, $1.72bn EAD), the engine reserves **$98.5m — a 5.75% coverage ratio.** The number that made me trust it: 5.75% sits *well below* the 19.8% lifetime default rate. That's correct, not a bug. Most loans are Stage 1, so only 12 months of risk are counted, and exposure amortizes down — so lifetime default risk lands *after* much of the balance is already repaid. Coverage then rises cleanly by stage (2.27% → 9.35% → 16.12%): worse stages must cost more per dollar, and they do. The macro overlay behaves too — adverse ECL comes in at roughly 2.2× baseline.

![IFRS 9 ECL report — evidence]({{ "/assets/basel-lite/ecl-report-evidence.png" | relative_url }})

A small thing I learned while wiring in the survival curve: Kaplan–Meier's tail is unreliable. Past a few years, almost no loans are still at risk, so KM extrapolates from a tiny sample and the cumulative default estimate inflates. It doesn't touch my ECL — the engine only uses the first 60 months (the longest loan term) and truncates the rest — but knowing *why* the tail is noisy, and that my design is robust to it by construction, is exactly the kind of detail that separates using a tool from understanding it.

---

## What I chose not to fake

The most important section. A prototype on static LendingClub data can't do everything a bank does, and pretending otherwise is the fastest way to lose credibility:

- **Staging is a proxy.** Real IFRS 9 staging tests for a *significant increase in credit risk since origination* — PD now vs PD at origination. Static data has no such history, so I stage on absolute PD + delinquency instead, and I say so on the report cover. It's an approximation, labelled as one.
- **The macro overlay is a prototype.** The Vasicek Z-factors are stress assumptions, not fitted from a real macro series. The honest upgrade — now unblocked, because I persisted the `issue_d` vintage column — is to regress vintage default rates on something like FRED unemployment.
- **LGD is portfolio-average**, EAD in the base EL is loan amount (the ECL engine improves on this), and grade/interest-rate partly encode LendingClub's own risk pricing.

---

## What I'd build next

Per-loan LGD instead of a portfolio average; a per-borrower Cox survival curve so *timing* varies by loan, not just level; a data-driven macro overlay fitted to unemployment by vintage; and a CI badge so the 16 tests run green on every push. And beyond Basel-Lite, the natural companion project: a **fraud / AML monitoring** system — imbalanced classification done with the same honesty.

---

## What it taught me

The modelling was the easy 30%. The real work — and the real learning — was in the layers that check and use the model: calibration that makes the loss math valid, validation that reports its own unflattering findings, and a provisioning engine that's transparent about which of its parts are real and which are approximations. That's the difference between a model and a system you'd actually let near capital.

*Code: [github.com/Ailya-Shah/basel_lite](https://github.com/Ailya-Shah/basel_lite). Built as part of CS-245, then taken well past the brief.*