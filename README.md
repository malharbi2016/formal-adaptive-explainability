# Adaptive Explainability Policies using PRISM-games

This repository contains the PRISM-games models and verification artifacts used in the study:

**“Design and Verification of Adaptive Explainability Policies in Service Robots under Uncertainty”**

The repository supports reproducibility, transparency, and formal verification of preference-parameterized adaptive explainability policies using stochastic multi-player games (SMGs).

The implementation evaluates explanation-quantity policies in a robot vacuum cleaner case study using two explanation levels: **minimal** and **comprehensive**. Human satisfaction and helpful-response outcomes are represented probabilistically, while task progression is deterministic in the current case-study implementation.

---

# Repository Structure

## `Models`

Contains the PRISM-games SMG models:

* `adaptive_model.prism`

  * Preference-parameterized adaptive model.
  * The information-preference parameter `w_info` controls the probability of selecting a comprehensive explanation:

    ```text
    P(comprehensive) = w_info
    P(minimal) = 1 - w_info
    ```

* `baseline_model.prism`

  * Non-adaptive randomized baseline.
  * Minimal and comprehensive explanations are selected with equal probability:

    ```text
    P(comprehensive) = 0.5
    P(minimal) = 0.5
    ```

The two models share the same human-response structure, satisfaction parameters, explanation-cost parameter, ten-round interaction horizon, reward structures, and terminal condition.

## `Properties`

Contains the PRISM-games verification properties:

* `rvc_properties_final.props`

  * Verification properties for the adaptive model.

* `rvc_baseline_properties_final.props`

  * Corresponding verification properties for the randomized baseline model.

---

# Model Parameters

The adaptive model uses the following parameters:

| Parameter    | Description                                                   | Nominal Value |
| ------------ | ------------------------------------------------------------- | ------------: |
| `w_info`     | Probability of selecting a comprehensive explanation          |           0.5 |
| `p_sat_comp` | Satisfaction probability after a comprehensive explanation    |           0.8 |
| `p_sat_min`  | Satisfaction probability after a minimal explanation          |           0.5 |
| `p_help`     | Probability of a helpful response conditional on satisfaction |           0.7 |
| `c_exp`      | Cost assigned to each comprehensive explanation               |           0.2 |

The baseline model uses the same parameters except for `w_info`, because its explanation-selection probabilities are fixed at `0.5/0.5`.

The parameter values used in the study are **illustrative modeling assumptions** intended to demonstrate the verification methodology and should not be interpreted as empirically estimated human-behavior parameters.

---

# Verification Objectives

The principal verification properties evaluate:

* Expected cumulative satisfaction-based utility
* Expected cumulative explanation cost
* Probability that the final response is helpful
* Probability that the user is satisfied in the final interaction round

The property files also contain auxiliary reward queries for:

* Expected number of helpful responses
* Expected number of satisfied interaction rounds
* Expected number of comprehensive explanations
* Expected number of minimal explanations

The principal PRISM-games queries are:

```text
R{"utility"}max=? [ F "done" ]

R{"cost"}min=? [ F "done" ]

Pmax=? [ F ("done" & Resp=1) ]

Pmax=? [ F ("done" & Satisfy=true) ]
```

All reward properties accumulate values until the model reaches the terminal state labeled `"done"`.

---

# Requirements

* PRISM-games 3.0 (Current release as of 2026: PRISM-games 3.2.4)
* Java 11+

PRISM-games can be obtained from:

https://www.prismmodelchecker.org/games/

---

# How to Run

## Adaptive Model

1. Open PRISM-games.

2. Load:

   ```text
   models/adaptive_model.prism
   ```

3. Build the model.

4. Specify values for the model constants. For the nominal configuration:

   ```text
   w_info = 0.5
   p_sat_comp = 0.8
   p_sat_min = 0.5
   p_help = 0.7
   c_exp = 0.2
   ```

5. Load:

   ```text
   properties/rvc_properties_final.props
   ```

6. Run the verification properties.

For the primary adaptive-policy experiments, vary `w_info` over the interval `[0,1]` while keeping the remaining parameters at their nominal values.

For example:

```text
w_info = 0.2
```

```text
w_info = 0.5
```

```text
w_info = 0.8
```

At `w_info = 0.5`, the adaptive model selects minimal and comprehensive explanations with equal probability and is therefore behaviorally equivalent to the randomized baseline.

## Baseline Model

1. Load:

   ```text
   models/baseline_model.prism
   ```

2. Specify the nominal parameter values:

   ```text
   p_sat_comp = 0.8
   p_sat_min = 0.5
   p_help = 0.7
   c_exp = 0.2
   ```

3. Load:

   ```text
   properties/rvc_baseline_properties_final.props
   ```

4. Run the verification properties.

The baseline always uses:

```text
P(comprehensive) = 0.5
P(minimal) = 0.5
```

---

# Representative Verification Results

Representative results using the nominal human-response and explanation-cost parameters are:

| Policy / Configuration   | Expected Utility | Expected Cost | P(Helpful Response) | P(Satisfaction) |
| ------------------------ | ---------------: | ------------: | ------------------: | --------------: |
| Adaptive, `w_info = 0.2` |             5.60 |          0.40 |               0.524 |           0.560 |
| Adaptive, `w_info = 0.5` |             6.50 |          1.00 |               0.560 |           0.650 |
| Adaptive, `w_info = 0.8` |             7.40 |          1.60 |               0.596 |           0.740 |
| Randomized baseline      |             6.50 |          1.00 |               0.560 |           0.650 |

As `w_info` increases, comprehensive explanations are selected more frequently. This increases expected satisfaction-based utility and response-related outcomes while also increasing explanation cost.

The adaptive policy therefore provides a **preference-dependent utility–cost trade-off** rather than uniformly outperforming the randomized baseline.

At the neutral configuration `w_info = 0.5`, the adaptive model and randomized baseline produce identical results, providing an internal consistency check between the two implementations.

---

# Reproducibility

The repository provides the adaptive and baseline SMG models, model parameter definitions, and corresponding verification property files required to reproduce the reported verification experiments.

The latest implementation is available on GitHub:

https://github.com/malharbi2016/formal-adaptive-explainability

A stable archived version is available through Zenodo:

https://doi.org/10.5281/zenodo.20273873

These artifacts support independent replication and allow the illustrative parameter values to be replaced with empirically derived values in future studies.

---

# Citation

If you use these models or verification artifacts, please cite the associated paper:

Mohammed Naji Alharbi, “Design and Verification of Adaptive Explainability Policies in Service Robots under Uncertainty.”

---

# License

Apache License 2.0
