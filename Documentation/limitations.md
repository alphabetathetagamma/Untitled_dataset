
# PRABASI Limitations

## 1. Administrative Boundary Changes

The administrative organization of India changed between 1991, 2001, and 2011.

Consequently, historical state-level observations are not always perfectly spatially comparable.

State-code harmonization addresses naming and coding differences where possible but cannot eliminate all historical boundary differences.

---

## 2. Jammu & Kashmir

Jammu & Kashmir presents an additional historical comparability issue because Census coverage and administrative definitions differ across the study periods.

The dataset does not claim perfect temporal equivalence for this region.

---

## 3. Missing-Value Replacement

Six Union Territory observations are replaced in:

`Girls marriage below 18 (%)`

The replacement uses the minimum observed value in that variable.

These values are therefore deterministic replacements and should not be treated as independently observed measurements.

---

## 4. Geographic Distance

Distance is calculated between state centroids.

It does not represent:
```
- road distance;
- railway distance;
- actual travel route;
- transportation-network distance;
- migration cost.
```

---

## 5. Aggregation

PRABASI operates on aggregated state/Union Territory-level observations.

It does not contain individual migration histories.

Therefore, individual-level migration behaviour cannot be inferred directly.

---

## 6. Causal Interpretation

The presence of a socio-economic indicator in the dataset does not imply a causal relationship between that indicator and migration.

Machine-learning associations and feature importance should be interpreted as predictive or associational rather than causal.

---

## 7. Source Dependence

The quality and comparability of PRABASI depend partly on the definitions, coverage, reference periods, and measurement procedures of the underlying source publications.