# Robust Spacecraft Attitude Estimation and Pointing Control Under Model and Sensor Uncertainty

**A SkySat-Class Agile Imager Study**  
**Repository:** `nexus-gnc`

---

## Context

Agile Earth-observation satellites in low Earth orbit must point their imagers to arcsecond-to-arcminute accuracy while slewing, using gyros and star trackers for estimation and reaction wheels for control. In practice, the estimator is tuned against imperfect noise models, the controller against an imperfect inertia, and sensors are intermittently unavailable (star-tracker blinding, outages and outliers). A filter that is accurate in nominal simulation can become inconsistent, and a controller that is stable with perfect state feedback can lose margin when driven by an uncertain estimate. These interactions — not the algorithms themselves — are what GNC engineers have to verify before flight.

## Reference Configuration

A representative agile imager:
- Reaction-wheel actuated, three-axis rigid body
- Sun-synchronous orbit at roughly 500 km
- Gyro + star tracker attitude sensing
- External disturbance torques

Mass properties, wheel torque and momentum limits, sensor noise levels and field of view: *[source needed — cite public mission or datasheet values; label anything else “assumed”]*.

## Problem

Quantify how model mismatch and sensor unavailability degrade a multiplicative extended Kalman filter (MEKF) and the closed-loop pointing it feeds, and determine which degradations the system tolerates, which it does not, and why.

## Central Questions

1. **Consistency** — Under what conditions does the MEKF remain statistically consistent (NEES, NIS, innovation whiteness), and when does it become overconfident or biased?
2. **Closed-loop impact** — How do estimation errors translate into pointing error, stability and jitter, compared with perfect-state feedback?
3. **Sensitivity** — How sensitive are accuracy and stability to errors in the process-noise (Q) and measurement-noise (R) tuning and to the inertia matrix?
4. **Sensor loss** — How does performance evolve through star-tracker outages and outlier measurements, and what are the observability limits during those periods?
5. **Mitigation** — How much degradation do outlier rejection, covariance management and adaptive tuning recover?
6. **Extension (Case B)** — How much additional pointing loss arises from temperature-dependent gyro bias and star-tracker misalignment, and how much do temperature-aware estimation variants recover? *(Include only if Case A is finished and verified.)*

## Scope

### In scope
- Nonlinear 3-axis rigid-body dynamics with reaction wheels and wheel limits
- Gyro model (noise, random walk, bias) and star-tracker model (noise, outages, outliers)
- Baseline MEKF (attitude and gyro bias), and optionally an EKF for contrast
- Quaternion-based nonlinear control and an LQR comparison, under perfect and estimated feedback
- Monte Carlo analysis with percentile reporting, observability analysis and an error budget

### Out of scope
- UKF, MPC and learning-based methods
- Orbit determination and relative navigation
- Flexible structures and wheel micro-vibration *(unless added as a clearly labelled extension)*

## Requirements
*[to be set and justified]*
- Pointing accuracy and stability requirement derived from the imager’s resolution and exposure assumptions
- Attitude knowledge requirement, with an error budget showing how it is allocated across sensors, estimator and control

## Approach

1. Define requirements and error budget
2. Verify each model against known-answer tests and analytical references
3. Establish the nominal MEKF as consistent before any degradation
4. Apply controlled degradations: Q/R mismatch, inertia error, sensor outage and outliers
5. Analyse observability for each condition and use it to explain the results
6. Evaluate mitigations, then (optionally) the thermal extension
7. Report 95th/99th percentile results with the run counts used

## Success Criteria

| ID | Criterion |
|----|-----------|
| **S1** | Nominal MEKF passes consistency tests across the Monte Carlo set *[thresholds to be defined]* |
| **S2** | Every reported result is traceable to a seeded, reproducible configuration |
| **S3** | For each degradation, the project states the operating region where requirements are met, the point where they fail, and the mechanism |
| **S4** | Reported mitigation benefits are shown with confidence intervals, not point values |
| **S5** | All parameters are cited or labelled “assumed”, with sensitivity analysis on the assumed ones |

## Planned Deliverables

- Requirements and error-budget document
- Verified simulation and estimator code with tests
- Monte Carlo and consistency reports
- Observability analysis
- Limitations section
- README whose headline figure shows pointing performance against degradation severity

## Status

**Early stage** — repository created, problem statement and success criteria defined. Implementation in progress.

---

*This is an independent project built on non-proprietary material. All parameters will be either publicly cited or explicitly labelled “assumed”.*
