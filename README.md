# Adaptive Explainability Policies using PRISM-games

This repository contains the PRISM-games models and verification artifacts used in the study:

> “Design and Verification of Adaptive Explainability Policies in Service Robots under Uncertainty”

The repository supports reproducibility, transparency, and formal verification of adaptive explainability policies in service robots operating under uncertainty using stochastic multi-player games (SMGs).

---

# Repository Structure

## models/

Contains the PRISM SMG models:

- `adaptive_model.prism`
  - Personality-aware adaptive policy using preference-weighted utility.

- `baseline_model.prism`
  - Non-adaptive baseline policy without preference-aware adaptation.

## properties/

Contains the verification queries:

- `rvc_properties_final.props`

---

# Verification Objectives

The provided properties evaluate:

- Expected cumulative utility
- Expected interaction cost
- Probability of helpful responses
- Probability of user satisfaction

using reward-based temporal verification in PRISM-games.

---

# Requirements

- PRISM-games 3.x
- Java 8+

Download:
https://www.prismmodelchecker.org/games/

---

# How to Run

1. Open PRISM-games.
2. Load a model from the `models/` directory.
3. Build the model.
4. Load `rvc_properties_final.props`.
5. Run verification queries.

For the adaptive model, define:

```text
w_info = 0.2
```

or

```text
w_info = 0.5
```

or

```text
w_info = 0.8
```

before verification.

---

# Verification Results

Representative verification outcomes:

| Configuration | Utility | Cost |
|---|---|---|
| w_info = 0.2 | 6.16 | 0.4 |
| w_info = 0.5 | 7.15 | 1.0 |
| w_info = 0.8 | 8.14 | 1.6 |

These results demonstrate the utility–cost trade-off achieved through adaptive explainability policies.

---

# Citation

If you use this repository, please cite the associated paper.

---

# License

Apache License 2.0
